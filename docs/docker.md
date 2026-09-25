# Docker — Development Setup

## Services

```yaml
mongo   # mongo:7            — port 27017, volume mongo_data
server  # node:20-alpine     — port 5000,  bind-mount ./server
client  # node:20-alpine     — port 3000,  bind-mount ./client
```

All services share the `kitchy_network` bridge network.  
`server` and `client` depend on `mongo`.

## Volumes

| Name       | Purpose                              |
|------------|--------------------------------------|
| mongo_data | MongoDB data persistence             |
| ./uploads  | Recipe images (bind-mount on server) |

## Environment variables

**server/.env**
```
PORT=5000
MONGO_URI=mongodb://mongo:27017/kitchy
JWT_SECRET=change_this_secret
NODE_ENV=development
```

**client/.env.local**
```
NEXT_PUBLIC_API_URL=http://localhost:5000
```

## Common commands

```bash
# Start all services
docker compose up

# Background mode
docker compose up -d

# Rebuild after dependency changes
docker compose up --build

# Stop (keeps volumes)
docker compose down

# Stop + wipe DB and uploads
docker compose down -v

# Follow logs for one service
docker compose logs -f server
```
