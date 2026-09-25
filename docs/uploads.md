# Uploads — Image Storage

## Multer config (`server/src/middleware/upload.js`)

- **Storage:** `diskStorage` → `server/uploads/`
- **Filename:** `<Date.now()>-<originalname>`
- **Accepted types:** `image/jpeg`, `image/png`, `image/webp`
- **Max size:** 5 MB

## Serving static files

In `server/src/index.js`:

```js
import { fileURLToPath } from 'url';
import path from 'path';

const __dirname = path.dirname(fileURLToPath(import.meta.url));

app.use('/uploads', express.static(path.join(__dirname, '../uploads')));
```

`__dirname` does not exist in ESM — rebuilt from `import.meta.url`.

## Docker volume

`./uploads` on the host is bind-mounted to `/app/uploads` in the server container.  
Images survive container restarts but are wiped with `docker compose down -v`.

## Frontend usage

```js
const imageUrl = `${process.env.NEXT_PUBLIC_API_URL}/uploads/${recipe.image}`;
```

The `image` field in the DB stores only the filename, not the full URL.
