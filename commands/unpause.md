---
name: unpause
description: Unpause paused containers
allowed-tools:
  - Bash
argument-hint: "<container> [container...]"
---

# Docker Unpause Command

Resume all processes in paused containers.

## Behavior

1. **Identify containers**: Find paused containers by name or ID
2. **Resume processes**: Unfreeze using SIGCONT
3. **Restore execution**: Processes continue from where they stopped
4. **Report**: Confirm containers resumed

## How It Works

- Sends SIGCONT to frozen processes
- Processes resume exactly where paused
- No state loss or restart
- Network connections resume
- Instant resumption

## Steps

1. Verify containers are paused
2. Execute: `docker unpause <container>`
3. Confirm running status
4. Verify processes resumed

## Examples

### Unpause Single Container
User: `/docker:unpause api`
→ `docker unpause api`
→ Resumes all processes in api container

### Unpause Multiple Containers
User: `/docker:unpause api worker scheduler`
→ `docker unpause api worker scheduler`
→ Resumes multiple containers

### Unpause All Paused
User: `/docker:unpause $(docker ps -qf "status=paused")`
→ Resumes all paused containers

## Use Cases

| Scenario | Action |
|----------|--------|
| After debugging | Resume normal operation |
| Resource available | Continue processing |
| Backup complete | Resume services |
| Testing done | Restore service |
| Queue ready | Resume batch processing |

## Workflow Example

### Debugging Workflow
```bash
# Pause to inspect state
docker pause api

# Examine container
docker inspect api
docker logs api
docker diff api

# Resume operation
docker unpause api
```

### Maintenance Window
```bash
# Pause non-critical services
docker pause worker scheduler

# Perform maintenance
# ...

# Resume services
docker unpause worker scheduler
```

### Resource Management
```bash
# Free up CPU temporarily
docker pause heavy-process

# Run critical task
docker run --rm critical-task

# Resume background process
docker unpause heavy-process
```

## Finding Paused Containers

```bash
# List all paused containers
docker ps --filter "status=paused"

# Get just IDs
docker ps -qf "status=paused"

# Check if specific container is paused
docker inspect --format='{{.State.Paused}}' <container>
```

## Error Handling

| Error | Cause | Solution |
|-------|-------|----------|
| "is not paused" | Container running | Already running, no action needed |
| "No such container" | Wrong name/ID | Check `docker ps -a` |
| "Container not running" | Container stopped | Use `docker start` instead |

## Tips

- Instant resumption, no restart delay
- Processes continue mid-execution
- Great for temporary resource management
- Use after `/docker:pause`
- Check status with `docker ps`
- Network clients may need to reconnect
