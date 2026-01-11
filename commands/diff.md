---
name: diff
description: Show changes to a container's filesystem
allowed-tools:
  - Bash
argument-hint: "<container>"
---

# Docker Diff Command

Show changes made to a container's filesystem since it started.

## Behavior

1. **Identify container**: Find by name or ID
2. **Compare filesystem**: Show differences from image
3. **Categorize changes**: Added, changed, or deleted files

## Change Types

| Symbol | Meaning |
|--------|---------|
| `A` | Added - File or directory was added |
| `C` | Changed - File or directory was modified |
| `D` | Deleted - File or directory was deleted |

## Steps

1. Verify container exists (running or stopped)
2. Execute: `docker diff <container>`
3. Display categorized file changes

## Examples

User: `/docker:diff api`
→ `docker diff api`
→ Shows all filesystem changes in api container

Output:
```
C /app
A /app/logs
A /app/logs/error.log
A /app/logs/access.log
C /etc/nginx/nginx.conf
D /tmp/setup.sh
```

User: `/docker:diff db`
→ Shows changes in database container
→ Useful to see what data files were created

## Use Cases

| Scenario | Benefit |
|----------|---------|
| Debug issues | See what files changed during execution |
| Audit changes | Check for unexpected modifications |
| Create image | Understand what to include in new layer |
| Security check | Detect unauthorized file changes |
| Data persistence | Identify files needing volume mounts |

## Common Patterns

### Application Logs
```
A /var/log/app/error.log
A /var/log/app/access.log
```
→ Consider mounting `/var/log/app` as volume

### Cache Files
```
A /root/.cache
A /tmp/cache
```
→ May want to clean in Dockerfile

### Config Changes
```
C /etc/app/config.json
```
→ Consider mounting config as volume

### Database Files
```
A /var/lib/postgresql/data
C /var/lib/postgresql/data/pg_wal
```
→ Must use volume for persistence

## Tips

- Works on running or stopped containers
- Useful before committing container to image
- Helps identify files for volume mounts
- Large number of changes may indicate issues
- Use with `/docker:cp` to extract changed files
- Compare before/after for debugging
