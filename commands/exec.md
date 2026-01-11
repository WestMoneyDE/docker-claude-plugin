---
name: exec
description: Execute commands inside a running container
allowed-tools:
  - Bash
argument-hint: "<container> [command] [--interactive]"
---

# Docker Exec Command

Execute commands inside running Docker containers.

## Behavior

1. **Identify container**: Find container by name or ID
2. **Determine mode**: Interactive shell or single command
3. **Execute**: Run command inside container
4. **Return output**: Show command results

## Common Usage

| Use Case | Command |
|----------|---------|
| Open shell | `docker exec -it <container> /bin/sh` |
| Run command | `docker exec <container> <command>` |
| As root | `docker exec -u root <container> <command>` |
| With env var | `docker exec -e VAR=value <container> <command>` |

## Steps

1. If no container specified, list running containers and ask which one
2. If no command specified, default to interactive shell (`/bin/sh` or `/bin/bash`)
3. For interactive use, add `-it` flags
4. Execute and show output

## Examples

User: `/docker:exec api`
→ `docker exec -it api /bin/sh`
→ Opens interactive shell in api container

User: `/docker:exec api ls -la /app`
→ `docker exec api ls -la /app`
→ Lists files in /app directory

User: `/docker:exec db psql -U postgres`
→ `docker exec -it db psql -U postgres`
→ Opens PostgreSQL shell

User: `/docker:exec nginx cat /etc/nginx/nginx.conf`
→ Shows nginx configuration file

## Tips

- Default to interactive mode for shells (sh, bash, zsh, psql, mysql, redis-cli)
- Try `/bin/sh` first, fall back to `/bin/bash` if available
- For Alpine-based images, use `/bin/sh` (no bash by default)
- Show helpful message if container is not running
