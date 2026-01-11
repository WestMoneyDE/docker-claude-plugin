---
name: run
description: Run Docker containers interactively
allowed-tools:
  - Bash
  - Read
argument-hint: "<image> [--port host:container] [--env KEY=VALUE] [--name container-name]"
---

# Docker Run Command

Run Docker containers with common options for local development.

## Behavior

1. **Parse options**: Extract ports, environment variables, name, and other flags
2. **Build command**: Construct appropriate `docker run` command
3. **Execute**: Run the container
4. **Report**: Show container status and access information

## Common Options

| Flag | Description |
|------|-------------|
| `-d` | Run in background (detached) |
| `-it` | Interactive with TTY (for shells) |
| `-p host:container` | Port mapping |
| `-e KEY=VALUE` | Environment variable |
| `--name` | Container name |
| `-v host:container` | Volume mount |
| `--rm` | Remove container when stopped |

## Steps

1. Parse the user's requirements
2. Construct the docker run command with appropriate flags
3. For web services, default to detached mode with port mapping
4. For interactive use, use `-it` flags
5. Execute and show results
6. If ports mapped, show how to access the service

## Examples

User: `/docker:run nginx`
→ `docker run -d --rm -p 80:80 --name nginx nginx`
→ Show: "Nginx running at http://localhost:80"

User: `/docker:run myapp --port 3000:3000`
→ `docker run -d --rm -p 3000:3000 --name myapp myapp`

User: `/docker:run postgres --env POSTGRES_PASSWORD=secret`
→ `docker run -d --rm -p 5432:5432 -e POSTGRES_PASSWORD=secret --name postgres postgres`

User: `/docker:run ubuntu` (for interactive shell)
→ `docker run -it --rm ubuntu /bin/bash`

## Tips

- Default to `--rm` for local development (auto-cleanup)
- Suggest common port mappings for known images
- Warn about missing required environment variables (e.g., POSTGRES_PASSWORD)
- For databases, suggest volume mounts for data persistence
