# OpenAPI — Swagger Documentation

**UI:** `http://localhost:5000/api-docs` (development only)  
**Spec format:** OpenAPI 3.0 via JSDoc annotations

## Dependencies

```bash
npm install swagger-jsdoc swagger-ui-express
```

## Setup — `server/src/config/swagger.js`

```js
import swaggerJsdoc from 'swagger-jsdoc';

const options = {
  definition: {
    openapi: '3.0.0',
    info: {
      title: 'Kitchy API',
      version: '1.0.0',
      description: 'Recipe management REST API',
    },
    servers: [{ url: 'http://localhost:5000' }],
    components: {
      securitySchemes: {
        bearerAuth: {
          type: 'http',
          scheme: 'bearer',
          bearerFormat: 'JWT',
        },
      },
      schemas: {
        Recipe: {
          type: 'object',
          properties: {
            _id:         { type: 'string' },
            title:       { type: 'string' },
            description: { type: 'string' },
            category:    { type: 'string', enum: ['breakfast','lunch','dinner','snack','dessert'] },
            prepTime:    { type: 'integer' },
            cookTime:    { type: 'integer' },
            servings:    { type: 'integer' },
            image:       { type: 'string' },
            tags:        { type: 'array', items: { type: 'string' } },
            author:      { type: 'string' },
          },
        },
        User: {
          type: 'object',
          properties: {
            _id:   { type: 'string' },
            name:  { type: 'string' },
            email: { type: 'string', format: 'email' },
          },
        },
        Error: {
          type: 'object',
          properties: {
            message: { type: 'string' },
          },
        },
      },
    },
  },
  apis: ['./src/routes/*.js'],
};

export default swaggerJsdoc(options);
```

## Mount in `server/src/index.js`

```js
import swaggerUi from 'swagger-ui-express';
import swaggerSpec from './config/swagger.js';

if (process.env.NODE_ENV !== 'production') {
  app.use('/api-docs', swaggerUi.serve, swaggerUi.setup(swaggerSpec));
}
```

## JSDoc annotation pattern

Annotations live in the route files (`server/src/routes/*.js`), not in controllers.

### Auth routes — `routes/auth.js`

```js
/**
 * @swagger
 * /api/auth/register:
 *   post:
 *     summary: Register a new user
 *     tags: [Auth]
 *     requestBody:
 *       required: true
 *       content:
 *         application/json:
 *           schema:
 *             type: object
 *             required: [name, email, password]
 *             properties:
 *               name:     { type: string }
 *               email:    { type: string, format: email }
 *               password: { type: string, minLength: 6 }
 *     responses:
 *       201:
 *         description: User created, returns JWT
 *         content:
 *           application/json:
 *             schema:
 *               type: object
 *               properties:
 *                 token: { type: string }
 *       400:
 *         description: Validation error or email already in use
 *         content:
 *           application/json:
 *             schema: { $ref: '#/components/schemas/Error' }
 */

/**
 * @swagger
 * /api/auth/login:
 *   post:
 *     summary: Login and obtain JWT
 *     tags: [Auth]
 *     requestBody:
 *       required: true
 *       content:
 *         application/json:
 *           schema:
 *             type: object
 *             required: [email, password]
 *             properties:
 *               email:    { type: string, format: email }
 *               password: { type: string }
 *     responses:
 *       200:
 *         description: Returns JWT
 *         content:
 *           application/json:
 *             schema:
 *               type: object
 *               properties:
 *                 token: { type: string }
 *       401:
 *         description: Invalid credentials
 *         content:
 *           application/json:
 *             schema: { $ref: '#/components/schemas/Error' }
 */

/**
 * @swagger
 * /api/auth/me:
 *   get:
 *     summary: Get current user profile
 *     tags: [Auth]
 *     security:
 *       - bearerAuth: []
 *     responses:
 *       200:
 *         description: User object
 *         content:
 *           application/json:
 *             schema:
 *               type: object
 *               properties:
 *                 data: { $ref: '#/components/schemas/User' }
 *       401:
 *         description: Unauthorized
 */
```

### Recipe routes — `routes/recipes.js`

```js
/**
 * @swagger
 * /api/recipes:
 *   get:
 *     summary: List recipes with optional search and filters
 *     tags: [Recipes]
 *     parameters:
 *       - in: query
 *         name: q
 *         schema: { type: string }
 *         description: Full-text search on title, description, tags
 *       - in: query
 *         name: category
 *         schema: { type: string, enum: [breakfast, lunch, dinner, snack, dessert] }
 *       - in: query
 *         name: maxPrepTime
 *         schema: { type: integer }
 *         description: Maximum prep time in minutes
 *       - in: query
 *         name: page
 *         schema: { type: integer, default: 1 }
 *       - in: query
 *         name: limit
 *         schema: { type: integer, default: 12 }
 *     responses:
 *       200:
 *         description: Paginated recipe list
 *         content:
 *           application/json:
 *             schema:
 *               type: object
 *               properties:
 *                 data:  { type: array, items: { $ref: '#/components/schemas/Recipe' } }
 *                 total: { type: integer }
 *                 page:  { type: integer }
 *
 *   post:
 *     summary: Create a new recipe
 *     tags: [Recipes]
 *     security:
 *       - bearerAuth: []
 *     requestBody:
 *       required: true
 *       content:
 *         multipart/form-data:
 *           schema:
 *             type: object
 *             required: [title]
 *             properties:
 *               title:       { type: string }
 *               description: { type: string }
 *               category:    { type: string }
 *               prepTime:    { type: integer }
 *               cookTime:    { type: integer }
 *               servings:    { type: integer }
 *               tags:        { type: string, description: 'Comma-separated' }
 *               image:       { type: string, format: binary }
 *     responses:
 *       201:
 *         description: Created recipe
 *         content:
 *           application/json:
 *             schema:
 *               type: object
 *               properties:
 *                 data: { $ref: '#/components/schemas/Recipe' }
 *       401:
 *         description: Unauthorized
 */

/**
 * @swagger
 * /api/recipes/{id}:
 *   get:
 *     summary: Get a single recipe
 *     tags: [Recipes]
 *     parameters:
 *       - in: path
 *         name: id
 *         required: true
 *         schema: { type: string }
 *     responses:
 *       200:
 *         description: Recipe object
 *         content:
 *           application/json:
 *             schema:
 *               type: object
 *               properties:
 *                 data: { $ref: '#/components/schemas/Recipe' }
 *       404:
 *         description: Not found
 *
 *   put:
 *     summary: Update a recipe (owner only)
 *     tags: [Recipes]
 *     security:
 *       - bearerAuth: []
 *     parameters:
 *       - in: path
 *         name: id
 *         required: true
 *         schema: { type: string }
 *     requestBody:
 *       content:
 *         multipart/form-data:
 *           schema:
 *             type: object
 *             properties:
 *               title:       { type: string }
 *               description: { type: string }
 *               category:    { type: string }
 *               prepTime:    { type: integer }
 *               cookTime:    { type: integer }
 *               servings:    { type: integer }
 *               tags:        { type: string }
 *               image:       { type: string, format: binary }
 *     responses:
 *       200:
 *         description: Updated recipe
 *       401:
 *         description: Unauthorized
 *       403:
 *         description: Forbidden — not the owner
 *       404:
 *         description: Not found
 *
 *   delete:
 *     summary: Delete a recipe (owner only)
 *     tags: [Recipes]
 *     security:
 *       - bearerAuth: []
 *     parameters:
 *       - in: path
 *         name: id
 *         required: true
 *         schema: { type: string }
 *     responses:
 *       200:
 *         description: Deleted successfully
 *       401:
 *         description: Unauthorized
 *       403:
 *         description: Forbidden — not the owner
 *       404:
 *         description: Not found
 */
```

## Tags summary

| Tag     | Routes          |
|---------|-----------------|
| Auth    | /api/auth/*     |
| Recipes | /api/recipes/*  |

## Testing JWT in Swagger UI

1. Open `http://localhost:5000/api-docs`
2. Call `POST /api/auth/login` and copy the returned `token`
3. Click **Authorize** (top right) → paste `<token>` (without "Bearer ")
4. All secured endpoints now include the header automatically
