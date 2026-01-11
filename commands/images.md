---
name: images
description: List Docker images on the system
allowed-tools:
  - Bash
argument-hint: "[--all] [--filter dangling=true] [repository]"
---

# Docker Images Command

List Docker images available on the local system.

## Behavior

1. **List images**: Show all or filtered images
2. **Format output**: Display in readable table
3. **Show details**: Size, tags, creation time

## Options

| Option | Description |
|--------|-------------|
| `--all, -a` | Show all images (including intermediate) |
| `--filter` | Filter by criteria (dangling, label, etc.) |
| `--digests` | Show image digests |
| `repository` | Filter by repository name |

## Steps

1. Run `docker images` with requested options
2. Format output with useful columns
3. Show total disk space used by images

## Examples

User: `/docker:images`
→ `docker images --format "table {{.Repository}}\t{{.Tag}}\t{{.Size}}\t{{.CreatedSince}}"`
→ Lists all images with size and age

User: `/docker:images node`
→ `docker images node`
→ Shows only node images

User: `/docker:images --filter dangling=true`
→ `docker images -f dangling=true`
→ Shows untagged/dangling images

User: `/docker:images --all`
→ `docker images -a`
→ Shows all images including intermediate layers

## Common Filters

| Filter | Description |
|--------|-------------|
| `dangling=true` | Untagged images |
| `label=key=value` | Images with specific label |
| `before=image` | Images created before |
| `since=image` | Images created after |
| `reference=pattern` | Images matching pattern |

## Output Columns

| Column | Description |
|--------|-------------|
| REPOSITORY | Image name |
| TAG | Image tag |
| IMAGE ID | Short ID |
| CREATED | When image was created |
| SIZE | Image size on disk |

## Tips

- Use with `/docker:clean` to remove unused images
- Dangling images waste disk space - clean regularly
- Show total disk usage: `docker system df`
- For detailed image info: `/docker:inspect <image>`
