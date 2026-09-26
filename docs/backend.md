# Backend — Express.js API

**Port:** 5000  
**Language:** TypeScript — compiled with `tsc`, source in `server/src/`, output in `server/dist/`  
**Entry point:** `server/src/index.ts`  
**Module system:** ES Modules — `"type": "module"` in `server/package.json`, `.js` extensions required in local imports (TypeScript emits `.js`)

## Folder structure

```
server/src/
├── config/        # DB connection, env validation
├── controllers/   # Route handler logic (no business logic in routes)
├── middleware/    # auth.ts, errorHandler.ts, upload.ts (multer)
├── models/        # Mongoose schemas
├── routes/        # Express routers mounted on /api
├── types/         # Shared TypeScript interfaces and type augmentations
└── index.ts
```

## API endpoints

| Method | Path                  | Auth | Description                     |
|--------|-----------------------|------|---------------------------------|
| POST   | /api/auth/register    | —    | Register user                   |
| POST   | /api/auth/login       | —    | Login, returns JWT              |
| GET    | /api/auth/me          | JWT  | Current user profile            |
| GET    | /api/recipes          | —    | List with search + filter       |
| POST   | /api/recipes          | JWT  | Create recipe + image upload    |
| GET    | /api/recipes/:id      | —    | Single recipe                   |
| PUT    | /api/recipes/:id      | JWT  | Update (owner only)             |
| DELETE | /api/recipes/:id      | JWT  | Delete (owner only)             |
| GET    | /uploads/:filename    | —    | Serve static image              |

## Query params — GET /api/recipes

| Param       | Type   | Description                        |
|-------------|--------|------------------------------------|
| q           | string | Full-text search on title/tags     |
| category    | string | Filter by category enum            |
| maxPrepTime | number | Max prep time in minutes           |
| page        | number | Pagination page (default: 1)       |
| limit       | number | Results per page (default: 12)     |

Interactive API docs available at `http://localhost:5000/api-docs` — see [openapi.md](openapi.md).

## Conventions

- Controllers return early on errors; all unhandled errors flow to `errorHandler.js`
- Ownership check: `recipe.author.toString() === req.user.id`
- All responses use `{ data }` on success, `{ message }` on error
