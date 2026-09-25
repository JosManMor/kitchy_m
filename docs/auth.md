# Auth — JWT + bcrypt

## Flow

1. **Register** — password hashed with `bcryptjs` (saltRounds: 10), user saved
2. **Login** — password compared with `bcrypt.compare`, JWT signed on match
3. **Protected routes** — `Authorization: Bearer <token>` header verified by `middleware/auth.js`

## Token

- **Library:** `jsonwebtoken`
- **Expiry:** 7 days
- **Payload:** `{ id: user._id }`
- **Secret:** `JWT_SECRET` env var (never hardcode)

## Middleware — `server/src/middleware/auth.js`

Attaches `req.user = { id }` from decoded token.  
Returns `401` if token is missing or invalid.

## Ownership guard

Routes that mutate a recipe verify the author before any DB write:

```js
if (recipe.author.toString() !== req.user.id) {
  return res.status(403).json({ message: 'Forbidden' });
}
```
