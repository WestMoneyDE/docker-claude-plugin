---
name: rmi
description: Remove one or more images
allowed-tools:
  - Bash
argument-hint: "<image> [image...] [--force] [--no-prune]"
---

# Docker RMI Command

Remove one or more Docker images.

## Behavior

1. **Identify images**: Find by name, tag, or ID
2. **Check usage**: Verify no containers using image
3. **Remove image**: Delete image and untagged parents
4. **Report results**: Show deleted image IDs

## Options

| Option | Description |
|--------|-------------|
| `-f, --force` | Force removal (even if in use) |
| `--no-prune` | Don't delete untagged parents |

## Steps

1. Verify image exists
2. Execute: `docker rmi <image>`
3. Show deleted image IDs
4. Report space reclaimed

## Examples

### Remove Single Image
User: `/docker:rmi myapp:v1`
→ `docker rmi myapp:v1`
→ Removes the myapp:v1 image

### Remove Multiple Images
User: `/docker:rmi myapp:v1 myapp:v2 myapp:v3`
→ `docker rmi myapp:v1 myapp:v2 myapp:v3`
→ Removes all specified images

### Force Remove
User: `/docker:rmi --force myapp:latest`
→ `docker rmi -f myapp:latest`
→ Force removes even if containers exist

### Remove by ID
User: `/docker:rmi abc123`
→ `docker rmi abc123`
→ Removes image by ID prefix

### Remove All Dangling Images
User: `/docker:rmi $(docker images -q -f dangling=true)`
→ Removes all untagged images

## Image Removal Patterns

### Remove All Images for Repository
```bash
# Remove all myapp images
docker rmi $(docker images myapp -q)

# Remove all versions of an image
docker rmi myapp:v1 myapp:v2 myapp:latest
```

### Remove Old Images
```bash
# List images by age
docker images --format "{{.Repository}}:{{.Tag}} {{.CreatedSince}}"

# Remove images older than pattern
docker rmi $(docker images --filter "before=myapp:current" -q)
```

### Remove Unused Images
```bash
# Remove dangling (untagged) images
docker image prune

# Remove all unused images
docker image prune -a

# Or use /docker:clean --all
```

## RMI vs Clean vs Prune

| Command | Removes | Use Case |
|---------|---------|----------|
| `rmi` | Specific images | Targeted cleanup |
| `image prune` | Dangling images | Quick cleanup |
| `image prune -a` | All unused | Aggressive cleanup |
| `/docker:clean` | All unused resources | Full system cleanup |

## Common Scenarios

### Clean Up Development Images
```bash
# Remove all dev/test versions
docker rmi myapp:dev myapp:test myapp:debug

# Keep only latest
docker rmi $(docker images myapp --filter "before=myapp:latest" -q)
```

### After Successful Deployment
```bash
# Remove old version after deploying new
docker rmi myapp:v1.0.0
docker rmi myapp:v1.0.1
# Keep: myapp:v1.0.2 (current)
```

### Clear Registry Cache
```bash
# Remove pulled images no longer needed
docker rmi nginx:1.24 node:18 python:3.11
```

## Error Handling

### Image In Use
```
Error: conflict: unable to remove (container X is using it)
```
**Solution**: Stop/remove containers first, or use `--force`

### Image Has Children
```
Error: conflict: unable to delete (has dependent child images)
```
**Solution**: Remove child images first, or use `--force`

### Image Not Found
```
Error: No such image: myapp:v1
```
**Solution**: Check image name with `docker images`

## Tips

- Use `docker images` to list images before removal
- Force removal may leave orphaned layers
- Removing by tag only untags; image deleted when no tags remain
- Use `/docker:clean` for bulk cleanup
- Image IDs can be abbreviated (first 3+ characters)
- Removing an image doesn't affect pushed copies in registries
- Use `--no-prune` to keep parent layers for other images
