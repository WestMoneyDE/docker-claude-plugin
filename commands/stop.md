---
name: stop
description: Stop running Docker containers
allowed-tools:
  - Bash
argument-hint: "<container> [--all] [--time seconds]"
---

# Docker Stop Command

Stop running Docker containers gracefully.

## Behavior

1. **Identify containers**: Find by name, ID, or stop all
2. **Send SIGTERM**: Allow graceful shutdown
3. **Wait for stop**: Default 10 seconds before SIGKILL
4. **Report**: Show stopped containers

## Options

| Option | Description |
|--------|-------------|
| `--all` | Stop all running containers |
| `--time, -t` | Seconds to wait before killing (default: 10) |

## Steps

1. If `--all` specified, get list of all running containers
2. If specific container, verify it exists and is running
3. Execute stop command with timeout if specified
4. Show confirmation of stopped containers

## Examples

User: `/docker:stop api`
→ `docker stop api`
→ Stops the api container

User: `/docker:stop api db redis`
→ `docker stop api db redis`
→ Stops multiple containers

User: `/docker:stop --all`
→ `docker stop $(docker ps -q)`
→ Stops all running containers

User: `/docker:stop api --time 30`
→ `docker stop -t 30 api`
→ Waits 30 seconds before force kill

## Related Commands

| Task | Command |
|------|---------|
| Stop and remove | `docker rm -f <container>` |
| Restart | `docker restart <container>` |
| Kill immediately | `docker kill <container>` |
| Pause | `docker pause <container>` |

## Tips

- Use `--time` for containers that need longer graceful shutdown
- After stopping, container can be restarted with `docker start`
- Use `/docker:clean` to remove stopped containers
- Show container status after stopping
