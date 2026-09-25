# Database — MongoDB + Mongoose

**Connection:** `mongodb://mongo:27017/kitchy` (via Docker service name)  
**ODM:** Mongoose 8.x

## Collections

### users

| Field     | Type   | Constraints        |
|-----------|--------|--------------------|
| name      | String | required           |
| email     | String | required, unique   |
| password  | String | required (hashed)  |
| createdAt | Date   | auto               |

### recipes

| Field       | Type     | Constraints                                        |
|-------------|----------|----------------------------------------------------|
| title       | String   | required                                           |
| description | String   | —                                                  |
| ingredients | Array    | [{ name, quantity, unit }]                         |
| steps       | [String] | ordered list                                       |
| category    | String   | enum: breakfast, lunch, dinner, snack, dessert     |
| prepTime    | Number   | minutes                                            |
| cookTime    | Number   | minutes                                            |
| servings    | Number   | —                                                  |
| image       | String   | relative path: `uploads/<filename>`                |
| author      | ObjectId | ref: User, required                                |
| tags        | [String] | —                                                  |
| createdAt   | Date     | auto                                               |
| updatedAt   | Date     | auto                                               |

## Indexes

- `users.email` — unique index
- `recipes.author` — for user profile queries
- `recipes.category` — for category filter
- `recipes` title + description — text index for `$text` search

## Volume

MongoDB data is persisted via the named Docker volume `mongo_data`.  
Dropping it with `docker compose down -v` wipes all data.
