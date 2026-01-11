---
name: create
description: Create a new container without starting it
allowed-tools:
  - Bash
  - Read
argument-hint: "<image> [options] [command]"
---

# Docker Create Command

Create a new container without starting it. Same options as `docker run`.

## Behavior

1. **Pull image**: Download if not present locally
2. **Create container**: Set up container with configuration
3. **Do not start**: Container remains in "created" state
4. **Return ID**: Output container ID

## Common Options

| Option | Description |
|--------|-------------|
| `--name` | Container name |
| `-p, --publish` | Port mapping |
| `-v, --volume` | Volume mount |
| `-e, --env` | Environment variable |
| `--network` | Network to connect |
| `--restart` | Restart policy |
| `-it` | Interactive with TTY |
| `--entrypoint` | Override entrypoint |

## Steps

1. Parse image and options
2. Execute: `docker create <options> <image>`
3. Return container ID
4. Show how to start with `docker start`

## Examples

### Basic Create
User: `/docker:create nginx`
→ `docker create nginx`
→ Creates nginx container, returns ID

### Create with Name and Ports
User: `/docker:create --name webserver -p 8080:80 nginx`
→ `docker create --name webserver -p 8080:80 nginx`
→ Creates named container with port mapping

### Create with Environment
User: `/docker:create --name db -e POSTGRES_PASSWORD=secret postgres:15`
→ Creates postgres container with password set

### Create with Volume
User: `/docker:create --name db -v pgdata:/var/lib/postgresql/data postgres:15`
→ Creates container with persistent volume

### Create with Full Config
User: `/docker:create --name api -p 3000:3000 -e NODE_ENV=production -v ./app:/app --restart unless-stopped myapp:latest`
→ Creates production-ready container

## Create vs Run

| Command | Creates | Starts | Use Case |
|---------|---------|--------|----------|
| `docker create` | Yes | No | Prepare container |
| `docker run` | Yes | Yes | Create and run |
| `docker run -d` | Yes | Yes (background) | Normal workflow |

## Container States

```
create → created → start → running → stop → exited
                     ↑                    ↓
                     └────────────────────┘
```

## Use Cases

### Prepare Before Starting
```bash
# Create container
docker create --name api myapp

# Copy files into it
docker cp config.json api:/app/config.json

# Start when ready
docker start api
```

### Batch Container Creation
```bash
# Create multiple containers
docker create --name web1 nginx
docker create --name web2 nginx
docker create --name web3 nginx

# Start all at once
docker start web1 web2 web3
```

### CI/CD Pipeline
```bash
# Create container
container_id=$(docker create myapp:$CI_SHA)

# Extract test artifacts
docker cp $container_id:/app/coverage ./coverage

# Cleanup
docker rm $container_id
```

### Copy Files from Image
```bash
# Create temporary container
docker create --name temp myapp

# Copy files out
docker cp temp:/app/dist ./dist

# Remove container
docker rm temp
```

### Pre-configure Container
```bash
# Create with all settings
docker create \
  --name production-api \
  --restart unless-stopped \
  --memory 1g \
  --cpus 2 \
  -p 3000:3000 \
  -e NODE_ENV=production \
  myapp:latest

# Start during deployment window
docker start production-api
```

## Viewing Created Containers

```bash
# List all containers including created
docker ps -a

# Filter by status
docker ps -a --filter "status=created"

# Show only created container IDs
docker ps -aq --filter "status=created"
```

## Tips

- Same options as `docker run`
- Useful for preparing containers before starting
- Can copy files into container before start
- Good for extracting files from images
- Container ID returned for scripting
- Use `docker start` to run created container
- Use `docker rm` to remove without starting
