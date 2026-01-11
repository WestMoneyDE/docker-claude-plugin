---
name: clean
description: Remove unused Docker images, containers, and volumes
allowed-tools:
  - Bash
argument-hint: "[--all] [--volumes] [--force]"
---

# Docker Clean Command

Clean up unused Docker resources to free disk space.

## Options

| Option | Description |
|--------|-------------|
| `--all, -a` | Remove all unused images, not just dangling |
| `--volumes` | Also remove unused volumes |
| `--force, -f` | Skip confirmation prompt |

## What Gets Cleaned

| Resource | Command | Description |
|----------|---------|-------------|
| Containers | `docker container prune` | Stopped containers |
| Images | `docker image prune` | Dangling images |
| Images (all) | `docker image prune -a` | All unused images |
| Volumes | `docker volume prune` | Unused volumes |
| Networks | `docker network prune` | Unused networks |
| Everything | `docker system prune` | All of the above |

## Behavior

1. **Show current usage**: Display disk space used by Docker
2. **Confirm cleanup**: Show what will be removed
3. **Execute**: Run appropriate prune commands
4. **Report**: Show space reclaimed

## Steps

1. First, show current disk usage:
   ```bash
   docker system df
   ```
2. Based on options, run appropriate cleanup:
   - Default: `docker system prune -f`
   - With `--all`: `docker system prune -a -f`
   - With `--volumes`: `docker system prune -a --volumes -f`
3. Show space reclaimed

## Examples

User: `/docker:clean`
→ Show disk usage, clean stopped containers and dangling images

User: `/docker:clean --all`
→ Remove all unused images (including tagged but unused)

User: `/docker:clean --volumes`
→ Also remove unused volumes (WARNING: data loss)

User: `/docker:clean --all --volumes`
→ Full cleanup of all unused resources

## Safety

- Always show what will be removed before cleaning
- Warn about volume removal (data loss risk)
- Suggest backing up data before `--volumes`
- Never remove running containers

## Tips

- Show disk space saved after cleanup
- Recommend running periodically
- Warn about removing images that may be needed
- For development, `--all` is usually safe
