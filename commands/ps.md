---
name: ps
description: List running Docker containers
allowed-tools:
  - Bash
argument-hint: "[--all] [--filter status=exited]"
---

# Docker PS Command

List Docker containers with status information.

## Options

| Option | Description |
|--------|-------------|
| `--all, -a` | Show all containers (including stopped) |
| `--filter` | Filter by status, name, etc. |
| `--format` | Custom output format |
| `--quiet, -q` | Only show container IDs |

## Behavior

1. **Run ps command**: Execute with appropriate filters
2. **Format output**: Display in readable table format
3. **Add context**: Show helpful information about each container

## Steps

1. Run `docker ps` with requested options
2. If no containers running, check for stopped containers
3. Format output with key information:
   - Container name
   - Image
   - Status
   - Ports
   - Created time

## Examples

User: `/docker:ps`
→ `docker ps --format "table {{.Names}}\t{{.Image}}\t{{.Status}}\t{{.Ports}}"`

User: `/docker:ps --all`
→ `docker ps -a --format "table {{.Names}}\t{{.Image}}\t{{.Status}}\t{{.Ports}}"`

User: `/docker:ps --filter status=exited`
→ Show only stopped containers

## Tips

- If no containers running, suggest starting one
- Highlight unhealthy containers
- Show port mappings clearly
- For stopped containers, show exit codes
