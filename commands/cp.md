---
name: cp
description: Copy files between container and local filesystem
allowed-tools:
  - Bash
argument-hint: "<src> <dest> (container:path or local path)"
---

# Docker CP Command

Copy files and directories between containers and the local filesystem.

## Behavior

1. **Parse paths**: Identify source and destination
2. **Determine direction**: Container→Host or Host→Container
3. **Execute copy**: Transfer files
4. **Verify**: Confirm successful copy

## Syntax

```
docker cp [OPTIONS] CONTAINER:SRC_PATH DEST_PATH    # Container to Host
docker cp [OPTIONS] SRC_PATH CONTAINER:DEST_PATH    # Host to Container
```

## Options

| Option | Description |
|--------|-------------|
| `-a, --archive` | Archive mode (preserve permissions) |
| `-L, --follow-link` | Follow symlinks in source |

## Steps

1. Parse source and destination paths
2. Identify container name and path
3. Execute: `docker cp <src> <dest>`
4. Verify files were copied

## Examples

### Container to Host
User: `/docker:cp api:/app/logs/error.log ./`
→ `docker cp api:/app/logs/error.log ./`
→ Copies error.log from container to current directory

User: `/docker:cp db:/var/lib/postgresql/data ./backup/`
→ Copies entire data directory

### Host to Container
User: `/docker:cp ./config.json api:/app/config.json`
→ `docker cp ./config.json api:/app/config.json`
→ Copies config file into container

User: `/docker:cp ./src api:/app/src`
→ Copies entire src directory into container

### With Options
User: `/docker:cp -a api:/app ./backup`
→ `docker cp -a api:/app ./backup`
→ Preserves file permissions and ownership

## Common Use Cases

| Scenario | Command |
|----------|---------|
| Extract logs | `docker cp api:/var/log/app.log ./` |
| Backup data | `docker cp db:/data ./backup/` |
| Update config | `docker cp ./nginx.conf web:/etc/nginx/` |
| Debug files | `docker cp api:/app/debug.txt ./` |
| Deploy hotfix | `docker cp ./fix.js api:/app/` |

## Path Rules

| Path | Meaning |
|------|---------|
| `container:/path` | Absolute path in container |
| `container:path` | Relative to container's workdir |
| `./local` | Relative to current directory |
| `/local/path` | Absolute local path |

## Tips

- Container can be running or stopped
- Use `-a` to preserve permissions for backups
- Copying directories copies contents recursively
- Target directory is created if it doesn't exist
- For large files, consider volume mounts instead
- Use `/docker:exec` to verify files after copy
