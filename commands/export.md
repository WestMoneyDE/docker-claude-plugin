---
name: export
description: Export a container's filesystem as a tar archive
allowed-tools:
  - Bash
argument-hint: "<container> -o <file.tar>"
---

# Docker Export Command

Export a container's filesystem as a tar archive.

## Behavior

1. **Identify container**: Find by name or ID
2. **Export filesystem**: Create flat tar of container's files
3. **Output archive**: Write to file or stdout
4. **No layers**: Single flat filesystem (no image history)

## Options

| Option | Description |
|--------|-------------|
| `-o, --output` | Write to file instead of stdout |

## Steps

1. Verify container exists
2. Execute: `docker export -o <file.tar> <container>`
3. Show archive size
4. Explain how to import

## Examples

### Export to File
User: `/docker:export api -o api-filesystem.tar`
→ `docker export -o api-filesystem.tar api`
→ Creates tar archive of container filesystem

### Export to Stdout
User: `/docker:export api > backup.tar`
→ `docker export api > backup.tar`
→ Pipes output to file

### Export with Compression
User: `/docker:export api | gzip > api.tar.gz`
→ Creates compressed archive

User: `/docker:export api | xz > api.tar.xz`
→ Creates highly compressed archive

## Export vs Save

| Feature | `export` | `save` |
|---------|----------|--------|
| Source | Container | Image |
| Output | Flat filesystem | Layered image |
| History | Lost | Preserved |
| Metadata | Lost | Preserved |
| Size | Often smaller | Includes all layers |
| Import | `docker import` | `docker load` |

```bash
# Export container filesystem (flat)
docker export api > api-fs.tar

# Save image with layers
docker save myapp:v1 > myapp-image.tar
```

## Use Cases

### Backup Container State
```bash
# Export current state
docker export api -o api-backup-$(date +%Y%m%d).tar

# Compress for storage
gzip api-backup-*.tar
```

### Create Minimal Image
```bash
# Export container (strips layers)
docker export api > api-flat.tar

# Import as new image (single layer)
docker import api-flat.tar myapp:minimal
```

### Extract Files for Analysis
```bash
# Export filesystem
docker export api -o api.tar

# Extract specific files
tar -xf api.tar app/logs
tar -xf api.tar etc/passwd

# Or inspect without extracting
tar -tvf api.tar | grep config
```

### Security Audit
```bash
# Export for scanning
docker export suspicious -o suspicious.tar

# Scan with security tools
tar -xf suspicious.tar -C ./audit/
# Run malware scanners, check for secrets, etc.
```

### Migration
```bash
# Export from source
docker export old-container -o migrate.tar

# Transfer to new host
scp migrate.tar user@newhost:/tmp/

# Import on new host
docker import /tmp/migrate.tar myapp:migrated
```

## Importing Exported Containers

```bash
# Basic import
docker import api.tar myapp:imported

# Import with changes
docker import --change "ENV DEBUG=true" api.tar myapp:debug

# Import from URL
docker import https://example.com/api.tar myapp:remote
```

## Archive Contents

The tar archive contains:
- Complete filesystem from container root
- All files and directories
- File permissions and ownership
- No Docker metadata or layers

```bash
# List archive contents
tar -tvf api.tar

# Check archive size
ls -lh api.tar

# Extract to directory
mkdir extracted
tar -xf api.tar -C extracted/
```

## Tips

- Works on running or stopped containers
- Flattens all layers into single filesystem
- Loses image history and metadata
- Great for creating minimal images
- Use `save` to preserve layers and history
- Useful for security audits and forensics
- Can import with `docker import` to create new image
