---
name: restart
description: Restart running Docker containers
allowed-tools:
  - Bash
argument-hint: "<container> [--time seconds]"
---

# Docker Restart Command

Restart running Docker containers.

## Behavior

1. **Identify container**: Find by name or ID
2. **Stop gracefully**: Send SIGTERM and wait
3. **Start again**: Bring container back up
4. **Report**: Show new container status

## Options

| Option | Description |
|--------|-------------|
| `--time, -t` | Seconds to wait before killing (default: 10) |

## Steps

1. Verify container exists
2. Execute restart: `docker restart <container>`
3. If timeout specified, add `-t` flag
4. Show container status after restart

## Examples

User: `/docker:restart api`
→ `docker restart api`
→ Restarts the api container

User: `/docker:restart api db`
→ `docker restart api db`
→ Restarts multiple containers

User: `/docker:restart api --time 30`
→ `docker restart -t 30 api`
→ Waits 30 seconds for graceful stop

User: `/docker:restart $(docker ps -q)`
→ Restarts all running containers

## When to Use

| Scenario | Command |
|----------|---------|
| Config change | Restart to pick up new env vars |
| Memory leak | Restart to free resources |
| Stuck process | Restart unresponsive container |
| After update | Restart with new image |

## Tips

- Restart preserves container configuration
- For compose services, use `/docker:compose restart`
- Check logs after restart if issues persist
- Consider `docker stop` + `docker start` for more control
