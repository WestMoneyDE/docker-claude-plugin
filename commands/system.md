---
name: system
description: Manage Docker system resources and show disk usage
allowed-tools:
  - Bash
argument-hint: "<df|prune|info|events> [options]"
---

# Docker System Command

Manage Docker system resources and view system-wide information.

## Operations

| Operation | Description |
|-----------|-------------|
| `df` | Show disk usage by Docker |
| `prune` | Remove all unused data |
| `info` | Display system information |
| `events` | Stream real-time events |

## Steps

1. Parse the operation
2. Execute appropriate docker system command
3. Format and display results

## Examples

### Disk Usage
User: `/docker:system df`
→ `docker system df`
→ Shows disk usage summary

User: `/docker:system df -v`
→ `docker system df -v`
→ Verbose output with details per item

### System Prune
User: `/docker:system prune`
→ `docker system prune`
→ Removes stopped containers, unused networks, dangling images

User: `/docker:system prune --all`
→ `docker system prune -a`
→ Also removes unused images (not just dangling)

User: `/docker:system prune --all --volumes`
→ `docker system prune -a --volumes`
→ Full cleanup including volumes (WARNING: data loss!)

### System Info
User: `/docker:system info`
→ `docker system info`
→ Shows Docker daemon configuration, resources, plugins

### Events
User: `/docker:system events`
→ `docker system events`
→ Streams real-time container/image/network events

User: `/docker:system events --filter type=container`
→ Filter to only container events

## Disk Usage Output

| Type | Description |
|------|-------------|
| Images | Space used by images |
| Containers | Space used by containers |
| Local Volumes | Space used by volumes |
| Build Cache | Space used by build cache |

## Prune Options

| Flag | Effect |
|------|--------|
| (default) | Stopped containers, unused networks, dangling images, build cache |
| `--all` | Add unused images (not just dangling) |
| `--volumes` | Add unused volumes |
| `--filter` | Filter what to prune |

## System Info Highlights

- Docker version and API version
- Number of containers (running/paused/stopped)
- Number of images
- Storage driver and backing filesystem
- CPU and memory available
- Kernel version
- Operating system

## Tips

- Run `df` before and after `prune` to see space saved
- Use `prune --all --volumes` carefully - it deletes data!
- Check `info` to verify Docker configuration
- Use `events` to debug container lifecycle issues
- Equivalent to `/docker:clean` but with more options
