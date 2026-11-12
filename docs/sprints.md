# Sprints — Development Plan

> **Available agents** (plugin `voltagent-core-dev`)
>
> | Agent | Short alias |
> |-------|-------------|
> | `voltagent-core-dev:backend-developer`   | backend |
> | `voltagent-core-dev:api-designer`        | api |
> | `voltagent-core-dev:frontend-developer`  | frontend |
> | `voltagent-core-dev:ui-designer`         | ui |
> | `voltagent-core-dev:fullstack-developer` | fullstack |

---

## Sprint 1 — Base Setup ✓

**Goal:** project running in Docker with MongoDB connection.  
**Branch:** `feat/base-setup`  
**Recommended agent:** `voltagent-core-dev:backend-developer`

- [x] Folder structure for `server/` and `client/`
- [x] `docker-compose.yml` with mongo, server, and client services
- [x] `server/package.json` with `"type": "module"`, TypeScript, and base dependencies
- [x] Mongoose connection in `src/config/db.ts`
- [x] Base Express app with `errorHandler` and health check `GET /api/health`
- [x] Environment variables (`.env.example`)

---

## Sprint 2 — Auth Module

**Goal:** register, login, and route protection with JWT.  
**Recommended agent:** `voltagent-core-dev:backend-developer`

- `User` model (Mongoose)
- `POST /api/auth/register`
- `POST /api/auth/login`
- `GET /api/auth/me`
- `auth.js` middleware (verifies JWT, attaches `req.user`)

---

## Sprint 3 — Recipes Module (CRUD)

**Goal:** full recipe operations with image upload.  
**Recommended agent:** `voltagent-core-dev:backend-developer`

- `Recipe` model (Mongoose)
- `GET /api/recipes/:id`
- `POST /api/recipes` + image upload with Multer
- `PUT /api/recipes/:id` (owner guard)
- `DELETE /api/recipes/:id` (owner guard)
- Serve static files at `/uploads`

---

## Sprint 4 — Search Module

**Goal:** paginated listing with search and filters.  
**Recommended agent:** `voltagent-core-dev:api-designer`

- `GET /api/recipes` with params `q`, `category`, `maxPrepTime`, `page`, `limit`
- MongoDB text index on title, description, and tags
- Pagination with `skip` / `limit`

---

## Sprint 5 — Frontend Auth

**Goal:** register and login flow in Next.js.  
**Recommended agent:** `voltagent-core-dev:frontend-developer`

- `/login` and `/register` pages
- Auth context with JWT in `localStorage`
- Protected route redirects
- Integration with Sprint 2 endpoints

---

## Sprint 6 — Frontend Recipes

**Goal:** complete recipe UI.  
**Recommended agents:** `voltagent-core-dev:frontend-developer` + `voltagent-core-dev:ui-designer`

- Home page `/` — recipe feed with search bar and filters
- `/recipes/[id]` — recipe detail
- `/recipes/new` — create recipe form with image upload (protected)
- `/profile` — user's own recipes (protected)

> Use `ui-designer` for the component system and Tailwind styles,  
> and `frontend-developer` for state logic and API integration.

---

## Sprint 7 — OpenAPI Docs

**Goal:** interactive API documentation available at `/api-docs`.  
**Recommended agent:** `voltagent-core-dev:api-designer`

- Install `swagger-jsdoc` + `swagger-ui-express`
- Configure `config/swagger.js`
- Annotate Auth routes (Sprint 2) with JSDoc
- Annotate Recipes routes (Sprint 3 and 4) with JSDoc
- Mount UI in `index.js` only for `NODE_ENV !== 'production'`
