---
name: stats
description: Display live resource usage statistics for containers
allowed-tools:
  - Bash
argument-hint: "[container] [--all] [--no-stream]"
---

# Docker Stats Command

Display live resource usage statistics for running containers.

## Behavior

1. **Identify targets**: Specific container(s) or all running
2. **Display stats**: Show CPU, memory, network, disk I/O
3. **Stream or snapshot**: Live updates or single reading

## Options

| Option | Description |
|--------|-------------|
| `--all, -a` | Show all containers (including stopped) |
| `--no-stream` | Single snapshot instead of live stream |
| `--format` | Custom output format |

## Output Columns

| Column | Description |
|--------|-------------|
| CONTAINER ID | Container identifier |
| NAME | Container name |
| CPU % | CPU usage percentage |
| MEM USAGE / LIMIT | Memory used / memory limit |
| MEM % | Memory usage percentage |
| NET I/O | Network bytes in / out |
| BLOCK I/O | Disk read / write |
| PIDS | Number of processes |

## Steps

1. If container specified, show stats for that container
2. Otherwise show all running containers
3. Default to streaming mode (live updates)
4. If `--no-stream`, show single snapshot

## Examples

User: `/docker:stats`
→ `docker stats`
→ Live stats for all running containers

User: `/docker:stats api`
→ `docker stats api`
→ Live stats for api container only

User: `/docker:stats --no-stream`
→ `docker stats --no-stream`
→ Single snapshot of all containers

User: `/docker:stats api db --no-stream`
→ Snapshot of specific containers

## Monitoring Use Cases

| Scenario | What to Check |
|----------|---------------|
| High CPU | CPU % column |
| Memory leak | MEM % increasing over time |
| Network issues | NET I/O for traffic |
| Disk bottleneck | BLOCK I/O for read/write |
| Process spawning | PIDS count |

## Tips

- Use `--no-stream` for scripting or quick checks
- High memory % near limit may cause OOM kills
- Compare CPU % against container's CPU limit
- Network I/O helps identify traffic patterns
- Use with `/docker:logs` to correlate performance issues
