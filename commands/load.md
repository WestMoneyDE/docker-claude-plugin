---
name: load
description: Load Docker images from a tar archive
allowed-tools:
  - Bash
argument-hint: "-i <file.tar> [--quiet]"
---

# Docker Load Command

Load Docker images from a tar archive created by `docker save`.

## Behavior

1. **Read archive**: Open tar file
2. **Extract layers**: Import all image layers
3. **Restore tags**: Apply original image tags
4. **Report**: Show loaded images

## Options

| Option | Description |
|--------|-------------|
| `-i, --input` | Read from tar file |
| `-q, --quiet` | Suppress output |

## Steps

1. Verify tar file exists
2. Execute: `docker load -i <file.tar>`
3. Show loaded images and tags
4. Verify with `docker images`

## Examples

### Load from File
User: `/docker:load -i myapp.tar`
→ `docker load -i myapp.tar`
→ Loads image(s) from archive

Output:
```
Loaded image: myapp:v1.0.0
```

### Load Compressed Archive
User: `/docker:load` with gzip file
→ `gunzip -c myapp.tar.gz | docker load`
→ Decompresses and loads

User: `/docker:load` with xz file
→ `xz -dc myapp.tar.xz | docker load`
→ Decompresses and loads

### Load Multiple Images
User: `/docker:load -i stack.tar`
→ Loads all images from archive

Output:
```
Loaded image: nginx:alpine
Loaded image: redis:latest
Loaded image: postgres:15
```

### Quiet Mode
User: `/docker:load -i myapp.tar --quiet`
→ `docker load -q -i myapp.tar`
→ Loads without output (for scripts)

## Use Cases

| Scenario | Command |
|----------|---------|
| Restore backup | `docker load -i backup.tar` |
| Offline install | `docker load -i airgap.tar` |
| CI/CD artifact | `docker load -i build.tar` |
| Team sharing | `docker load -i shared-images.tar` |
| Migration | `docker load -i migration.tar` |

## Transfer Workflow

### Receive and Load
```bash
# Receive file (scp, USB, download, etc.)
scp user@source:/tmp/myapp.tar.gz ./

# Decompress
gunzip myapp.tar.gz

# Load into Docker
docker load -i myapp.tar

# Verify
docker images myapp
```

### One-liner for Compressed
```bash
# gzip
gunzip -c myapp.tar.gz | docker load

# xz
xz -dc myapp.tar.xz | docker load

# zstd
zstd -dc myapp.tar.zst | docker load

# From URL
curl -sL https://example.com/myapp.tar.gz | gunzip | docker load
```

## Air-Gapped Deployment

### Prepare Package
```bash
# On connected machine
docker pull nginx:alpine
docker pull redis:latest
docker save -o stack.tar nginx:alpine redis:latest
```

### Deploy Offline
```bash
# On air-gapped machine
docker load -i stack.tar
docker images
```

## Load vs Pull

| Feature | `docker load` | `docker pull` |
|---------|---------------|---------------|
| Source | Local tar file | Registry |
| Network | Not required | Required |
| Use case | Offline, backup | Normal workflow |
| Speed | Depends on disk | Depends on network |

## Troubleshooting

| Error | Cause | Solution |
|-------|-------|----------|
| `open: no such file` | Wrong path | Check file path |
| `invalid tar header` | Corrupted file | Re-transfer file |
| `unexpected EOF` | Incomplete file | Check file size |
| Decompression error | Wrong format | Match decompress tool |

## Tips

- Restores exact image with original tags
- Layers already present are skipped
- Use `-q` for scripted deployments
- Verify loaded images: `docker images`
- Works with archives from any Docker version
- Use `/docker:save` to create archives
