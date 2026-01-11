---
name: pause
description: Pause all processes in running containers
allowed-tools:
  - Bash
argument-hint: "<container> [container...]"
---

# Docker Pause Command

Pause all processes within one or more running containers.

## Behavior

1. **Identify containers**: Find by name or ID
2. **Freeze processes**: Suspend all processes using SIGSTOP
3. **Maintain state**: Container stays running but frozen
4. **Report**: Confirm containers paused

## How It Works

- Uses cgroups freezer to suspend processes
- Container remains in "running" state but paused
- Memory and state preserved
- Network connections maintained (but unresponsive)
- No CPU usage while paused

## Steps

1. Verify containers are running
2. Execute: `docker pause <container>`
3. Confirm paused status
4. Show how to unpause

## Examples

### Pause Single Container
User: `/docker:pause api`
→ `docker pause api`
→ Freezes all processes in api container

### Pause Multiple Containers
User: `/docker:pause api worker scheduler`
→ `docker pause api worker scheduler`
→ Freezes multiple containers

### Pause All Running
User: `/docker:pause $(docker ps -q)`
→ Pauses all running containers

## Use Cases

| Scenario | Benefit |
|----------|---------|
| Debugging | Freeze state for inspection |
| Resource management | Temporarily free CPU |
| Snapshot preparation | Consistent state for backup |
| Testing | Simulate service unavailability |
| Batch processing | Queue work while paused |

## Container States

```
running → pause → paused → unpause → running
    ↓                                    ↑
  stop ──────────────────────────────────┘
```

## Pause vs Stop

| Feature | `pause` | `stop` |
|---------|---------|--------|
| Processes | Frozen | Terminated |
| Memory | Preserved | Released |
| State | Maintained | Lost (unless saved) |
| Resume | Instant | Requires restart |
| CPU usage | None | None |
| Network | Connected (frozen) | Disconnected |

## Checking Paused Status

```bash
# List paused containers
docker ps --filter "status=paused"

# Check specific container
docker inspect --format='{{.State.Paused}}' <container>
```

## Tips

- Use `/docker:unpause` to resume
- Paused containers show "Paused" in `docker ps`
- Cannot exec into paused container
- Health checks will fail while paused
- Network timeouts may occur for connected clients
- Great for debugging race conditions
