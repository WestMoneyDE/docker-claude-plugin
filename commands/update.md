---
name: update
description: Update container resource limits and restart policy
allowed-tools:
  - Bash
argument-hint: "<container> [--memory] [--cpus] [--restart]"
---

# Docker Update Command

Update configuration of running or stopped containers without recreating them.

## Behavior

1. **Identify container**: Find by name or ID
2. **Apply updates**: Modify resource limits or policies
3. **Live update**: Changes apply immediately (most settings)
4. **Report**: Confirm new configuration

## Options

| Option | Description |
|--------|-------------|
| `--memory, -m` | Memory limit (e.g., 512m, 1g) |
| `--memory-swap` | Total memory + swap limit |
| `--cpus` | Number of CPUs (e.g., 1.5) |
| `--cpu-shares` | CPU shares (relative weight) |
| `--cpu-period` | CPU CFS period |
| `--cpu-quota` | CPU CFS quota |
| `--restart` | Restart policy |
| `--pids-limit` | Max number of PIDs |
| `--blkio-weight` | Block IO weight (10-1000) |

## Steps

1. Verify container exists
2. Execute: `docker update <options> <container>`
3. Confirm changes applied
4. Verify with `docker inspect`

## Examples

### Update Memory Limit
User: `/docker:update api --memory 1g`
→ `docker update --memory 1g api`
→ Sets memory limit to 1GB

User: `/docker:update api --memory 512m --memory-swap 1g`
→ Sets memory to 512MB with 1GB swap

### Update CPU Limit
User: `/docker:update api --cpus 2`
→ `docker update --cpus 2 api`
→ Limits to 2 CPU cores

User: `/docker:update api --cpus 0.5`
→ Limits to half a CPU core

### Update Restart Policy
User: `/docker:update api --restart always`
→ `docker update --restart always api`
→ Always restart container

User: `/docker:update api --restart unless-stopped`
→ Restart unless manually stopped

User: `/docker:update api --restart on-failure:5`
→ Restart on failure, max 5 attempts

### Update Multiple Containers
User: `/docker:update --memory 512m api worker scheduler`
→ Updates all three containers

## Restart Policies

| Policy | Behavior |
|--------|----------|
| `no` | Never restart (default) |
| `always` | Always restart |
| `unless-stopped` | Restart unless manually stopped |
| `on-failure[:max]` | Restart on non-zero exit, optional max retries |

## Memory Formats

| Format | Example | Meaning |
|--------|---------|---------|
| bytes | `536870912` | 512MB |
| kilobytes | `524288k` | 512MB |
| megabytes | `512m` | 512MB |
| gigabytes | `1g` | 1GB |

## Use Cases

| Scenario | Command |
|----------|---------|
| Fix OOM issues | `docker update --memory 2g api` |
| Limit CPU hog | `docker update --cpus 1 worker` |
| Enable auto-restart | `docker update --restart always api` |
| Reduce resources | `docker update --memory 256m --cpus 0.5 test` |
| Production hardening | `docker update --restart unless-stopped --memory 1g api` |

## Live Update Behavior

| Setting | Live Update | Notes |
|---------|-------------|-------|
| Memory | Yes | Immediate effect |
| CPU | Yes | Immediate effect |
| Restart policy | Yes | Applies on next exit |
| PIDs limit | Yes | Immediate effect |

## Verification

```bash
# Check memory limit
docker inspect --format='{{.HostConfig.Memory}}' api

# Check CPU limit
docker inspect --format='{{.HostConfig.NanoCpus}}' api

# Check restart policy
docker inspect --format='{{.HostConfig.RestartPolicy.Name}}' api

# Full resource config
docker inspect --format='{{json .HostConfig}}' api | jq
```

## Tips

- Most updates apply without restart
- Memory limits require kernel support
- Use `docker stats` to monitor effect
- Restart policy applies on next container exit
- Cannot update all settings (ports, volumes need recreate)
- Works on running and stopped containers
