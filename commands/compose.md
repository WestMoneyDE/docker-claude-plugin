---
name: compose
description: Docker Compose operations (up, down, restart, logs)
allowed-tools:
  - Bash
  - Read
  - Glob
argument-hint: "<up|down|restart|logs|ps> [service-name] [options]"
---

# Docker Compose Command

Manage multi-container applications with Docker Compose.

## Operations

| Operation | Description |
|-----------|-------------|
| `up` | Start all services (detached by default) |
| `down` | Stop and remove containers |
| `restart` | Restart services |
| `logs` | View service logs |
| `ps` | List running services |

## Behavior

1. **Find compose file**: Look for docker-compose.yml or compose.yml
2. **Parse operation**: Determine which action to perform
3. **Execute**: Run appropriate docker compose command
4. **Report**: Show status and helpful information

## Steps

1. Check for docker-compose.yml or compose.yml in current directory
2. If not found, search in common locations or ask user
3. Execute the requested operation
4. Show relevant output and next steps

## Examples

User: `/docker:compose up`
→ `docker compose up -d`
→ Show running services and ports

User: `/docker:compose down`
→ `docker compose down`
→ Confirm services stopped

User: `/docker:compose logs api`
→ `docker compose logs -f api`
→ Stream logs from api service

User: `/docker:compose restart`
→ `docker compose restart`
→ Show restarted services

User: `/docker:compose ps`
→ `docker compose ps`
→ Show service status table

## Additional Options

- `up --build` - Rebuild images before starting
- `up --force-recreate` - Recreate containers even if unchanged
- `down -v` - Also remove volumes
- `logs --tail 100` - Show last 100 lines

## Tips

- Default `up` to detached mode (-d) for local development
- For `logs`, default to follow mode (-f)
- After `up`, show which ports are mapped
- If compose file has issues, help diagnose and fix
