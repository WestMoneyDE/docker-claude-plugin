---
name: rename
description: Rename a Docker container
allowed-tools:
  - Bash
argument-hint: "<container> <new_name>"
---

# Docker Rename Command

Rename an existing Docker container.

## Behavior

1. **Identify container**: Find by current name or ID
2. **Validate new name**: Check name is valid and available
3. **Rename**: Apply new name to container
4. **Report**: Confirm rename successful

## Steps

1. Verify container exists
2. Check new name is not in use
3. Execute: `docker rename <old_name> <new_name>`
4. Confirm with `docker ps`

## Examples

### Basic Rename
User: `/docker:rename quirky_einstein api`
→ `docker rename quirky_einstein api`
→ Renames auto-generated name to meaningful name

### Rename Running Container
User: `/docker:rename old-api new-api`
→ `docker rename old-api new-api`
→ Works on running containers

### Rename by ID
User: `/docker:rename abc123 myapp`
→ `docker rename abc123 myapp`
→ Rename using container ID

## Naming Rules

Valid container names must:
- Start with alphanumeric character
- Contain only `[a-zA-Z0-9_.-]`
- Be unique among all containers

```bash
# Valid names
api
my-app
web_server
app.v1

# Invalid names
-api        # Can't start with dash
my app      # No spaces
api:v1      # No colons
```

## Use Cases

| Scenario | Example |
|----------|---------|
| Fix auto-name | `docker rename quirky_einstein api` |
| Version update | `docker rename api api-v1` |
| Environment label | `docker rename db prod-db` |
| Standardize naming | `docker rename Container1 frontend` |
| Pre-migration | `docker rename api api-old` |

## Common Workflows

### After Docker Run Without Name
```bash
# Started without --name
docker run -d nginx
# Container gets random name like "quirky_einstein"

# Rename to something meaningful
docker rename quirky_einstein webserver
```

### Blue-Green Deployment
```bash
# Current production
docker rename api api-blue

# Deploy new version
docker run -d --name api-green myapp:v2

# Switch (via load balancer)
# ...

# Cleanup old
docker rm api-blue
```

### Migration Preparation
```bash
# Rename old container
docker rename api api-backup

# Start new container with original name
docker run -d --name api myapp:new

# Test, then remove backup
docker rm api-backup
```

## Error Handling

| Error | Cause | Solution |
|-------|-------|----------|
| "No such container" | Wrong name/ID | Check `docker ps -a` |
| "name is already in use" | Name taken | Choose different name or remove existing |
| "Invalid container name" | Bad characters | Use only `[a-zA-Z0-9_.-]` |

## Tips

- Works on running and stopped containers
- Does not affect container ID
- Network aliases remain unchanged
- Compose may override on restart
- Use meaningful names from the start with `--name`
- Renamed containers keep all settings
