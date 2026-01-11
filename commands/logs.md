---
name: logs
description: View and follow container logs
allowed-tools:
  - Bash
argument-hint: "<container-name> [--tail N] [--since time] [--follow]"
---

# Docker Logs Command

View logs from running or stopped containers.

## Options

| Option | Description |
|--------|-------------|
| `--follow, -f` | Stream logs in real-time |
| `--tail N` | Show last N lines |
| `--since` | Show logs since timestamp |
| `--timestamps, -t` | Show timestamps |

## Behavior

1. **Identify container**: Find container by name or ID
2. **Apply options**: Set tail count, follow mode, etc.
3. **Stream logs**: Display log output

## Steps

1. If container name not provided, list running containers and ask which one
2. Default to showing last 100 lines with follow mode
3. Execute: `docker logs --tail 100 -f <container>`
4. If container not found, show available containers

## Examples

User: `/docker:logs api`
→ `docker logs --tail 100 -f api`

User: `/docker:logs nginx --tail 50`
→ `docker logs --tail 50 -f nginx`

User: `/docker:logs db --since 1h`
→ `docker logs --since 1h -f db`

User: `/docker:logs` (no container specified)
→ List running containers, ask which one to show logs for

## Tips

- Default to follow mode for active debugging
- If logs are large, suggest using `--tail`
- For timestamps, add `-t` flag
- Help identify error patterns in logs
