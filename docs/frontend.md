# Frontend — Next.js

**Framework:** Next.js 14 (App Router)  
**Port:** 3000  
**API base:** `NEXT_PUBLIC_API_URL=http://localhost:5000`

## Pages

```
app/
├── page.tsx                # Home — recipe feed + search bar
├── recipes/
│   ├── [id]/page.tsx       # Recipe detail
│   └── new/page.tsx        # Create recipe form (protected)
├── profile/page.tsx        # User's own recipes (protected)
├── login/page.tsx
└── register/page.tsx
```

## Auth state

- JWT stored in `localStorage`
- Auth context provided via React Context API
- Protected pages redirect to `/login` if no token

## Key dependencies

| Package         | Purpose                        |
|-----------------|--------------------------------|
| axios           | HTTP requests to API           |
| react-hook-form | Form state management          |
| zod             | Schema validation              |
| tailwindcss     | Utility-first styling          |

## Image handling

Recipe images are served from `NEXT_PUBLIC_API_URL/uploads/<filename>`.  
Always prepend the API URL — paths stored in the DB are relative.
