---
name: volume
description: Manage Docker volumes for persistent data
allowed-tools:
  - Bash
argument-hint: "<ls|create|rm|inspect|prune> [name] [options]"
---

# Docker Volume Command

Manage Docker volumes for persistent data storage.

## Operations

| Operation | Description |
|-----------|-------------|
| `ls` | List volumes |
| `create` | Create a new volume |
| `rm` | Remove a volume |
| `inspect` | Show volume details |
| `prune` | Remove unused volumes |

## Steps

1. Parse the operation and arguments
2. Execute appropriate docker volume command
3. Show results and storage information

## Examples

### List Volumes
User: `/docker:volume ls`
→ `docker volume ls`
→ Shows all volumes

User: `/docker:volume ls --filter dangling=true`
→ Shows unused volumes

### Create Volume
User: `/docker:volume create mydata`
→ `docker volume create mydata`
→ Creates named volume

User: `/docker:volume create --driver local --opt type=nfs mydata`
→ Creates NFS volume

### Remove Volume
User: `/docker:volume rm mydata`
→ `docker volume rm mydata`
→ Removes the volume (must not be in use)

### Inspect Volume
User: `/docker:volume inspect mydata`
→ Shows volume details, mount point, driver

### Prune Unused
User: `/docker:volume prune`
→ Removes all unused volumes
→ WARNING: This deletes data!

## Using Volumes

### Mount in Container
```bash
# Named volume
docker run -v mydata:/app/data myimage

# Anonymous volume
docker run -v /app/data myimage

# Bind mount (host path)
docker run -v /host/path:/container/path myimage
```

### In Docker Compose
```yaml
volumes:
  mydata:

services:
  db:
    volumes:
      - mydata:/var/lib/postgresql/data
```

## Volume vs Bind Mount

| Feature | Volume | Bind Mount |
|---------|--------|------------|
| Location | Docker managed | Host filesystem |
| Portability | High | Low |
| Backup | `docker run --volumes-from` | Direct file access |
| Performance | Optimized | Varies |
| Use case | Production data | Development |

## Common Tasks

| Task | Command |
|------|---------|
| List all volumes | `docker volume ls` |
| Find unused volumes | `docker volume ls -f dangling=true` |
| Check volume size | `docker system df -v` |
| Backup volume | `docker run --rm -v mydata:/data -v $(pwd):/backup alpine tar cvf /backup/backup.tar /data` |

## Tips

- Always use named volumes for important data
- Volumes persist after container removal
- Use `/docker:clean --volumes` to remove unused volumes
- Back up volumes before pruning
- Check volume usage with `docker system df`
