---
name: start
description: Start stopped containers
allowed-tools:
  - Bash
argument-hint: "<container> [--attach] [--interactive]"
---

# Docker Start Command

Start one or more stopped containers.

## Behavior

1. **Identify containers**: Find by name or ID
2. **Start container**: Resume from stopped state
3. **Optionally attach**: Connect to container output
4. **Report**: Show container status

## Options

| Option | Description |
|--------|-------------|
| `--attach, -a` | Attach to container and show output |
| `--interactive, -i` | Attach stdin for interactive mode |

## Steps

1. Verify container exists and is stopped
2. Execute: `docker start <container>`
3. If attach requested, show output
4. Confirm container started

## Examples

User: `/docker:start api`
→ `docker start api`
→ Starts stopped api container

User: `/docker:start api db worker`
→ `docker start api db worker`
→ Starts multiple containers

User: `/docker:start api --attach`
→ `docker start -a api`
→ Starts and attaches to output

User: `/docker:start myshell --interactive`
→ `docker start -ai myshell`
→ Starts with interactive terminal

## Start vs Run

| Command | Behavior |
|---------|----------|
| `docker run` | Create NEW container and start |
| `docker start` | Start EXISTING stopped container |

## Container Lifecycle

```
create → start → running → stop → stopped → start → running
                    ↓                           ↑
                  kill ─────────────────────────┘
                    ↓
                   rm → removed
```

## Common Workflows

### Restart Stopped Container
```bash
docker start api
```

### Start and View Logs
```bash
docker start api
docker logs -f api
```

### Start with Attached Output
```bash
docker start -a api
```

### Start All Stopped Containers
```bash
docker start $(docker ps -aq -f status=exited)
```

## Related Commands

| Task | Command |
|------|---------|
| Stop container | `/docker:stop` |
| Restart container | `/docker:restart` |
| Create new | `/docker:run` |
| List stopped | `/docker:ps --all` |

## Tips

- Preserves all container configuration
- Faster than creating new container
- Use `--attach` to see startup output
- Use `-ai` for interactive containers
- Check logs if container exits immediately
- Container keeps its name, ID, and volumes
