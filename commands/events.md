---
name: events
description: Get real-time events from the Docker daemon
allowed-tools:
  - Bash
argument-hint: "[--filter] [--since] [--until] [--format]"
---

# Docker Events Command

Get real-time events from the Docker daemon.

## Behavior

1. **Connect to daemon**: Stream events in real-time
2. **Filter events**: By type, container, image, etc.
3. **Display events**: Show timestamp, type, action, actor
4. **Historical events**: Query past events with --since/--until

## Options

| Option | Description |
|--------|-------------|
| `-f, --filter` | Filter events by criteria |
| `--since` | Show events since timestamp |
| `--until` | Show events until timestamp |
| `--format` | Format output using Go template |

## Steps

1. Execute: `docker events [options]`
2. Stream events to terminal (Ctrl+C to stop)
3. Explain event types and actions

## Examples

### Stream All Events
User: `/docker:events`
→ `docker events`
→ Streams all Docker events in real-time

### Filter by Container
User: `/docker:events --filter container=api`
→ `docker events --filter container=api`
→ Shows only events for 'api' container

### Filter by Event Type
User: `/docker:events --filter type=container`
→ `docker events --filter type=container`
→ Shows only container events

### Filter by Action
User: `/docker:events --filter event=start`
→ `docker events --filter event=start`
→ Shows only start events

### Historical Events
User: `/docker:events --since 1h`
→ `docker events --since 1h`
→ Shows events from the last hour

### Time Range
User: `/docker:events --since 2024-01-01 --until 2024-01-02`
→ Shows events for that date range

## Event Types

| Type | Description |
|------|-------------|
| `container` | Container lifecycle events |
| `image` | Image operations |
| `volume` | Volume operations |
| `network` | Network operations |
| `daemon` | Daemon events |
| `plugin` | Plugin events |
| `node` | Swarm node events |
| `service` | Swarm service events |
| `secret` | Swarm secret events |
| `config` | Swarm config events |

## Container Events

| Action | Description |
|--------|-------------|
| `create` | Container created |
| `start` | Container started |
| `stop` | Container stopped |
| `kill` | Container killed |
| `die` | Container exited |
| `pause` | Container paused |
| `unpause` | Container unpaused |
| `restart` | Container restarted |
| `attach` | Attached to container |
| `detach` | Detached from container |
| `exec_create` | Exec instance created |
| `exec_start` | Exec instance started |
| `destroy` | Container removed |

## Image Events

| Action | Description |
|--------|-------------|
| `pull` | Image pulled |
| `push` | Image pushed |
| `tag` | Image tagged |
| `untag` | Image untagged |
| `delete` | Image deleted |
| `import` | Image imported |
| `save` | Image saved |
| `load` | Image loaded |

## Filter Options

| Filter | Description | Example |
|--------|-------------|---------|
| `container` | Container name or ID | `--filter container=api` |
| `event` | Event action | `--filter event=start` |
| `image` | Image name | `--filter image=nginx` |
| `label` | Container label | `--filter label=env=prod` |
| `type` | Event type | `--filter type=container` |
| `volume` | Volume name | `--filter volume=data` |
| `network` | Network name | `--filter network=bridge` |

## Common Use Cases

### Monitor Container Lifecycle
```bash
# Watch all container events
docker events --filter type=container

# Watch specific container
docker events --filter container=api

# Watch for restarts (might indicate problems)
docker events --filter event=restart
```

### Debug Container Issues
```bash
# Watch for container deaths
docker events --filter event=die

# Check what happened to a container
docker events --filter container=api --since 1h

# Monitor OOM kills
docker events --filter event=oom
```

### Monitor Deployments
```bash
# Watch for new containers
docker events --filter event=create --filter event=start

# Watch for image pulls
docker events --filter type=image --filter event=pull
```

### Audit Activity
```bash
# All events in the last 24 hours
docker events --since 24h --until 0s

# Export events for analysis
docker events --since 24h --format '{{json .}}' > events.json
```

## Custom Format

```bash
# Simple format
docker events --format '{{.Time}} {{.Type}} {{.Action}} {{.Actor.Attributes.name}}'

# JSON format for parsing
docker events --format '{{json .}}'

# Table format
docker events --format 'table {{.Time}}\t{{.Type}}\t{{.Action}}'
```

## Time Formats

```bash
# Relative time
docker events --since 30m    # Last 30 minutes
docker events --since 2h     # Last 2 hours
docker events --since 1d     # Last day (some versions)

# Unix timestamp
docker events --since 1704067200

# RFC 3339 format
docker events --since 2024-01-01T00:00:00Z

# Date only
docker events --since 2024-01-01
```

## Tips

- Events stream continuously until Ctrl+C
- Use `--since` to query historical events
- Combine with `--until 0s` to get past events and exit
- JSON format useful for log aggregation
- Filter multiple values: `--filter event=start --filter event=stop`
- Great for debugging container crashes
- Use with `watch` or cron for monitoring
- Events include container labels in Actor.Attributes
