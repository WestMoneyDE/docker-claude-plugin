---
name: rm
description: Remove stopped containers
allowed-tools:
  - Bash
argument-hint: "<container> [--force] [--volumes]"
---

# Docker RM Command

Remove one or more stopped containers.

## Behavior

1. **Identify containers**: Find by name or ID
2. **Verify stopped**: Container must be stopped (unless --force)
3. **Remove**: Delete container and optionally volumes
4. **Report**: Confirm removal

## Options

| Option | Description |
|--------|-------------|
| `--force, -f` | Force remove running container |
| `--volumes, -v` | Remove associated anonymous volumes |

## Steps

1. Verify container exists
2. Check if stopped (or use --force)
3. Execute: `docker rm <container>`
4. Confirm container removed

## Examples

User: `/docker:rm api`
→ `docker rm api`
→ Removes stopped api container

User: `/docker:rm api db worker`
→ `docker rm api db worker`
→ Removes multiple containers

User: `/docker:rm api --force`
→ `docker rm -f api`
→ Force removes running container (stops first)

User: `/docker:rm api --volumes`
→ `docker rm -v api`
→ Also removes anonymous volumes

User: `/docker:rm $(docker ps -aq)`
→ Removes all stopped containers

## Remove Patterns

| Task | Command |
|------|---------|
| Remove one | `docker rm <container>` |
| Remove multiple | `docker rm c1 c2 c3` |
| Force remove | `docker rm -f <container>` |
| Remove with volumes | `docker rm -v <container>` |
| Remove all stopped | `docker rm $(docker ps -aq -f status=exited)` |
| Remove all | `docker rm -f $(docker ps -aq)` |

## Related Commands

| Task | Command |
|------|---------|
| Stop then remove | `docker stop api && docker rm api` |
| Stop and remove | `docker rm -f api` |
| Remove image | `/docker:clean` or `docker rmi` |
| Remove volumes | `/docker:volume rm` |
| Remove all unused | `/docker:system prune` |

## Tips

- Must stop container first (or use --force)
- Anonymous volumes are NOT removed by default
- Use `--volumes` to clean up anonymous volumes
- Named volumes are never auto-removed
- Use `/docker:clean` for bulk cleanup
- Force remove is equivalent to stop + rm
