---
name: import
description: Import contents from a tarball to create a filesystem image
allowed-tools:
  - Bash
argument-hint: "<file|URL> [repository[:tag]] [--change] [--message]"
---

# Docker Import Command

Import the contents from a tarball to create a filesystem image.

## Behavior

1. **Read tarball**: From file, URL, or stdin
2. **Create image**: Single-layer filesystem image
3. **Apply changes**: Optional Dockerfile instructions
4. **Tag image**: Apply repository and tag name

## Options

| Option | Description |
|--------|-------------|
| `-c, --change` | Apply Dockerfile instruction |
| `-m, --message` | Commit message for import |
| `--platform` | Set platform (os/arch) |

## Steps

1. Verify tarball exists (file) or is accessible (URL)
2. Execute: `docker import <file> <image:tag>`
3. Show new image ID
4. Verify with `docker images`

## Examples

### Import from File
User: `/docker:import backup.tar myapp:restored`
→ `docker import backup.tar myapp:restored`
→ Creates image from tarball

### Import from URL
User: `/docker:import https://example.com/rootfs.tar myapp:remote`
→ `docker import https://example.com/rootfs.tar myapp:remote`
→ Downloads and imports

### Import from Stdin
User: `/docker:import - myapp:piped < backup.tar`
→ `cat backup.tar | docker import - myapp:piped`
→ Imports from stdin

### Import with Changes
User: `/docker:import backup.tar myapp:v1 --change "ENV NODE_ENV=production"`
→ `docker import -c "ENV NODE_ENV=production" backup.tar myapp:v1`
→ Imports and sets environment

### Import with Multiple Changes
User: `/docker:import backup.tar myapp:v1 --change "EXPOSE 8080" --change "CMD [\"node\", \"app.js\"]"`
→ Imports with port and command set

## Change Instructions

Supported Dockerfile instructions with `--change`:

| Instruction | Example |
|-------------|---------|
| `CMD` | `--change 'CMD ["node", "app.js"]'` |
| `ENTRYPOINT` | `--change 'ENTRYPOINT ["/start.sh"]'` |
| `ENV` | `--change 'ENV NODE_ENV=production'` |
| `EXPOSE` | `--change 'EXPOSE 8080'` |
| `LABEL` | `--change 'LABEL version=1.0'` |
| `USER` | `--change 'USER appuser'` |
| `WORKDIR` | `--change 'WORKDIR /app'` |
| `VOLUME` | `--change 'VOLUME /data'` |

## Import vs Load

| Feature | `import` | `load` |
|---------|----------|--------|
| Source | Tarball (flat filesystem) | Docker save archive |
| Layers | Creates single layer | Preserves all layers |
| History | Lost | Preserved |
| Metadata | Lost | Preserved |
| Pair with | `docker export` | `docker save` |

```bash
# Export/Import workflow (flat filesystem)
docker export container > backup.tar
docker import backup.tar myapp:imported

# Save/Load workflow (preserves layers)
docker save myapp:v1 > myapp.tar
docker load < myapp.tar
```

## Use Cases

### Restore from Export
```bash
# Previously exported container
docker export api > api-backup.tar

# Restore as new image
docker import api-backup.tar api:restored

# Run restored image
docker run -d --name api-new api:restored
```

### Create Minimal Image
```bash
# Export running container (strips layers)
docker export myapp > myapp-flat.tar

# Import as minimal single-layer image
docker import myapp-flat.tar myapp:minimal

# Compare sizes
docker images myapp
```

### Import with Configuration
```bash
# Import with production settings
docker import backup.tar myapp:prod \
  -c "ENV NODE_ENV=production" \
  -c "EXPOSE 3000" \
  -c "CMD [\"node\", \"server.js\"]"
```

### Create Base Image
```bash
# Create minimal rootfs
mkdir rootfs
# ... add files ...
tar -C rootfs -cvf rootfs.tar .

# Import as base image
docker import rootfs.tar mybase:v1
```

### Import from Remote
```bash
# Import from URL
docker import https://example.com/images/app.tar myapp:remote

# Import from S3 (with aws cli)
aws s3 cp s3://bucket/app.tar - | docker import - myapp:s3
```

## Working with Compressed Archives

```bash
# Import gzipped tarball
gunzip -c backup.tar.gz | docker import - myapp:v1

# Import xz compressed
xz -dc backup.tar.xz | docker import - myapp:v1

# Or decompress first
gunzip backup.tar.gz
docker import backup.tar myapp:v1
```

## Migration Workflow

```bash
# On source machine
docker export old-container -o migrate.tar
scp migrate.tar user@newhost:/tmp/

# On destination machine
docker import /tmp/migrate.tar myapp:migrated
docker run -d --name new-container myapp:migrated
```

## Tips

- Import creates a single-layer image (no history)
- Use `--change` to set CMD, ENTRYPOINT, ENV, etc.
- Imported images have no parent layers
- Works with tarballs from `docker export` or custom rootfs
- URL imports support HTTP/HTTPS
- Use `docker load` for images from `docker save`
- Compressed archives must be decompressed first (or piped)
- Great for creating minimal images or custom base images
