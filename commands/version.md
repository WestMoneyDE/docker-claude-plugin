---
name: version
description: Show Docker version information
allowed-tools:
  - Bash
argument-hint: "[--format]"
---

# Docker Version Command

Show Docker version information for client and server.

## Behavior

1. **Query versions**: Get client and server version info
2. **Display details**: Show version, API, Go version, OS/Arch
3. **Check compatibility**: Verify client/server API compatibility

## Options

| Option | Description |
|--------|-------------|
| `-f, --format` | Format output using Go template |

## Steps

1. Execute: `docker version`
2. Display client and server versions
3. Note any version mismatches or compatibility issues

## Examples

### Show Full Version
User: `/docker:version`
→ `docker version`
→ Shows complete version info for client and server

### Client Version Only
User: `/docker:version --format '{{.Client.Version}}'`
→ Shows just the client version number

### Server Version Only
User: `/docker:version --format '{{.Server.Version}}'`
→ Shows just the server version number

### JSON Output
User: `/docker:version --format '{{json .}}'`
→ Outputs version info as JSON

## Output Fields

### Client Information
| Field | Description |
|-------|-------------|
| Version | Docker client version |
| API version | Client API version |
| Go version | Go compiler version |
| Git commit | Source commit hash |
| Built | Build timestamp |
| OS/Arch | Client platform |

### Server Information
| Field | Description |
|-------|-------------|
| Engine Version | Docker Engine version |
| API version | Server API version |
| Min API version | Minimum supported API |
| Go version | Go compiler version |
| Git commit | Source commit hash |
| Built | Build timestamp |
| OS/Arch | Server platform |

## Common Format Templates

```bash
# Just version numbers
docker version --format '{{.Client.Version}} / {{.Server.Version}}'

# API versions
docker version --format 'Client API: {{.Client.APIVersion}}, Server API: {{.Server.APIVersion}}'

# Platform info
docker version --format '{{.Client.Os}}/{{.Client.Arch}}'

# Check if server is running
docker version --format '{{.Server.Version}}' 2>/dev/null && echo "Docker is running"
```

## Version vs Info

| Command | Shows |
|---------|-------|
| `docker version` | Version numbers, API versions, build info |
| `docker info` | System config, storage driver, plugins, resources |

```bash
# Version info
docker version

# System info (more detailed)
docker info

# Or use /docker:system info
```

## Troubleshooting

### Cannot Connect to Docker Daemon
```
Cannot connect to the Docker daemon at unix:///var/run/docker.sock
```
**Solutions**:
- Start Docker: `sudo systemctl start docker`
- Check permissions: `sudo usermod -aG docker $USER`
- Docker Desktop: Ensure it's running

### Client/Server Version Mismatch
```
Client: 24.0.0
Server: 20.10.0
```
**Note**: Usually works if API versions are compatible. Update both for best experience.

### API Version Negotiation
```bash
# Check API versions
docker version --format '{{.Client.APIVersion}} vs {{.Server.APIVersion}}'

# Force specific API version
DOCKER_API_VERSION=1.41 docker version
```

## Use Cases

### Verify Installation
```bash
# Quick check Docker is working
docker version

# Just confirm it's running
docker version --format '{{.Server.Version}}' && echo "OK"
```

### Check Compatibility
```bash
# Before running commands that need specific features
docker version --format '{{.Server.Version}}'

# Example: BuildKit requires 18.09+
```

### Scripting
```bash
# Get version for scripts
VERSION=$(docker version --format '{{.Server.Version}}')
echo "Running Docker $VERSION"

# Check minimum version
MIN_VERSION="20.10"
CURRENT=$(docker version --format '{{.Server.Version}}')
# Compare versions...
```

### Documentation
```bash
# Include in bug reports
docker version

# Or compact format
docker version --format 'Client: {{.Client.Version}}, Server: {{.Server.Version}}'
```

## Tips

- Run without options for full version info
- Client version shown even if daemon not running
- Server version requires running daemon
- API version determines available features
- Use `docker info` for system configuration details
- Version mismatch usually not a problem if APIs compatible
- Great first command to verify Docker installation
