---
name: commit
description: Create a new image from a container's changes
allowed-tools:
  - Bash
argument-hint: "<container> [repository[:tag]] [--change] [--message]"
---

# Docker Commit Command

Create a new image from a container's changes.

## Behavior

1. **Identify container**: Find by name or ID
2. **Capture changes**: Include all filesystem modifications
3. **Create image**: Save as new image layer
4. **Tag image**: Apply repository and tag name

## Options

| Option | Description |
|--------|-------------|
| `-a, --author` | Author (e.g., "Name <email>") |
| `-c, --change` | Apply Dockerfile instruction |
| `-m, --message` | Commit message |
| `-p, --pause` | Pause container during commit (default: true) |

## Steps

1. Verify container exists
2. Execute: `docker commit <container> <image:tag>`
3. Show new image ID
4. Verify with `docker images`

## Examples

### Basic Commit
User: `/docker:commit api myapp:modified`
→ `docker commit api myapp:modified`
→ Creates image from api container

### Commit with Message
User: `/docker:commit api myapp:v2 --message "Added config files"`
→ `docker commit -m "Added config files" api myapp:v2`

### Commit with Author
User: `/docker:commit api myapp:v2 --author "Dev <dev@example.com>"`
→ `docker commit -a "Dev <dev@example.com>" api myapp:v2`

### Commit with Changes
User: `/docker:commit api myapp:v2 --change "ENV NODE_ENV=production"`
→ `docker commit -c "ENV NODE_ENV=production" api myapp:v2`

User: `/docker:commit api myapp:v2 --change "EXPOSE 8080" --change "CMD [\"node\", \"app.js\"]"`
→ Applies multiple Dockerfile instructions

## Change Instructions

Supported Dockerfile instructions with `--change`:

| Instruction | Example |
|-------------|---------|
| `CMD` | `--change 'CMD ["node", "app.js"]'` |
| `ENTRYPOINT` | `--change 'ENTRYPOINT ["/start.sh"]'` |
| `ENV` | `--change 'ENV NODE_ENV=production'` |
| `EXPOSE` | `--change 'EXPOSE 8080'` |
| `LABEL` | `--change 'LABEL version=1.0'` |
| `USER` | `--change 'USER appuser'` |
| `WORKDIR` | `--change 'WORKDIR /app'` |
| `VOLUME` | `--change 'VOLUME /data'` |

## Use Cases

### Save Manual Configuration
```bash
# Start container and configure manually
docker run -it --name mysetup ubuntu bash
# Install packages, edit files, etc.
exit

# Save as new image
docker commit mysetup myapp:configured
```

### Debug Image Creation
```bash
# Run and modify container
docker run -d --name debug myapp
docker exec debug apt-get update
docker exec debug apt-get install -y vim

# Save debug version
docker commit debug myapp:debug
```

### Quick Prototype
```bash
# Start base image
docker run -it --name proto python:3.12 bash
# pip install packages, create files
exit

# Commit for later use
docker commit proto myproto:v1
```

### Hotfix Image
```bash
# Fix running container
docker exec api vim /app/config.json

# Create hotfix image
docker commit api myapp:hotfix-001

# Deploy hotfix
docker stop api
docker run -d --name api-fixed myapp:hotfix-001
```

## Commit vs Dockerfile

| Feature | `commit` | Dockerfile |
|---------|----------|------------|
| Reproducible | No | Yes |
| Documented | Partial | Full |
| Automation | Manual | CI/CD ready |
| Use case | Quick saves, debugging | Production |

**Best practice:** Use Dockerfiles for production, `commit` for debugging and prototyping.

## Viewing Commit History

```bash
# Show image history
docker history myapp:modified

# Show image details
docker inspect myapp:modified
```

## Tips

- Container is paused during commit by default
- Use `--pause=false` for live systems (risk of inconsistency)
- Committed images can be pushed to registries
- Use meaningful tags for versioning
- Document changes with `--message`
- Prefer Dockerfiles for reproducible builds
- Great for debugging and quick experiments
