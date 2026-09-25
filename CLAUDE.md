# Kitchy — Recipe App

## Stack

Next.js · Express.js · MongoDB · Docker (development)

## Structure

```
kitchy/
├── client/          # Next.js frontend
├── server/          # Express.js REST API
├── uploads/         # Recipe images (Docker volume)
├── docs/            # Project documentation → see docs/README.md
└── docker-compose.yml
```

## Services

| Service | Port | Description          |
|---------|------|----------------------|
| mongo   | 27017| MongoDB database     |
| server  | 5000 | Express REST API     |
| client  | 3000 | Next.js dev server   |

## Language

All code, comments, commits, branch names, and documentation must be written in **English**.

## Git Conventions

Follows [Conventional Commits](https://www.conventionalcommits.org/).

### Commit format

```
<type>(<scope>): <short description>
```

| Type       | When to use                              |
|------------|------------------------------------------|
| `feat`     | New feature                              |
| `fix`      | Bug fix                                  |
| `docs`     | Documentation only                       |
| `chore`    | Maintenance, deps, config                |
| `refactor` | Code change without feature or fix       |
| `test`     | Adding or updating tests                 |
| `style`    | Formatting, no logic change              |

**Examples:**
```
feat(auth): add JWT middleware
fix(recipes): correct owner guard on delete
docs(openapi): annotate recipes routes
chore(server): add swagger-jsdoc dependency
```

### Branch naming

```
<type>/<short-description>
```

**Examples:**
```
feat/auth-module
feat/recipes-crud
fix/upload-path-esm
docs/openapi-annotations
chore/docker-setup
```

## Documentation

All detailed documentation lives in [`docs/`](docs/README.md).  
Start there for backend, database, frontend, auth, and Docker references.
