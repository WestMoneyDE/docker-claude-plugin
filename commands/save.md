---
name: save
description: Save Docker images to a tar archive
allowed-tools:
  - Bash
argument-hint: "<image> [image...] -o <file.tar>"
---

# Docker Save Command

Save one or more Docker images to a tar archive for offline transfer or backup.

## Behavior

1. **Identify images**: Find images by name or ID
2. **Create archive**: Package images with all layers
3. **Output file**: Write to specified tar file
4. **Report**: Show archive size and contents

## Options

| Option | Description |
|--------|-------------|
| `-o, --output` | Write to file instead of stdout |

## Steps

1. Verify images exist locally
2. Execute: `docker save -o <file.tar> <image>`
3. Show archive file size
4. List included images

## Examples

### Save Single Image
User: `/docker:save myapp:v1 -o myapp.tar`
→ `docker save -o myapp.tar myapp:v1`
→ Creates myapp.tar with the image

### Save Multiple Images
User: `/docker:save nginx:alpine redis:latest -o stack.tar`
→ `docker save -o stack.tar nginx:alpine redis:latest`
→ Creates archive with both images

### Save All Tags
User: `/docker:save myapp -o myapp-all.tar`
→ Saves all tags of myapp image

### Compressed Archive
User: `/docker:save myapp:v1 | gzip > myapp.tar.gz`
→ Creates compressed archive

User: `/docker:save myapp:v1 | xz > myapp.tar.xz`
→ Creates highly compressed archive

## Use Cases

| Scenario | Command |
|----------|---------|
| Offline transfer | `docker save -o image.tar myapp` |
| Backup image | `docker save -o backup.tar myapp:prod` |
| Share with team | `docker save -o share.tar myapp:latest` |
| Air-gapped systems | `docker save -o airgap.tar app db cache` |
| CI artifact | `docker save -o build.tar myapp:$CI_SHA` |

## Transfer Workflow

### On Source Machine
```bash
# Save image to tar
docker save -o myapp.tar myapp:v1.0.0

# Optional: compress
gzip myapp.tar
# Result: myapp.tar.gz

# Transfer via scp, USB, etc.
scp myapp.tar.gz user@remote:/tmp/
```

### On Target Machine
```bash
# Decompress if needed
gunzip myapp.tar.gz

# Load image
docker load -i myapp.tar
```

## Archive Contents

The tar archive includes:
- All image layers
- Image metadata
- Tags and labels
- Parent image references

```bash
# Inspect archive contents
tar -tvf myapp.tar
```

## Compression Comparison

| Format | Command | Compression | Speed |
|--------|---------|-------------|-------|
| tar | `docker save -o img.tar` | None | Fastest |
| gzip | `docker save \| gzip > img.tar.gz` | Good | Fast |
| xz | `docker save \| xz > img.tar.xz` | Best | Slow |
| zstd | `docker save \| zstd > img.tar.zst` | Great | Fast |

## Tips

- Includes all layers, can be large
- Use compression for transfer over network
- Multiple images can share layers in archive
- Use `/docker:load` to restore images
- Check image size first: `docker images <name>`
- Archive preserves tags and metadata
