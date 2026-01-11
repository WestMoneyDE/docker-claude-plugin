---
name: search
description: Search Docker Hub for images
allowed-tools:
  - Bash
argument-hint: "<term> [--filter] [--limit] [--no-trunc]"
---

# Docker Search Command

Search Docker Hub for images.

## Behavior

1. **Query Docker Hub**: Search for matching images
2. **Display results**: Show name, description, stars, official status
3. **Filter results**: Apply filters for official, automated, stars
4. **Limit output**: Control number of results

## Options

| Option | Description |
|--------|-------------|
| `-f, --filter` | Filter output (stars, is-official, is-automated) |
| `--format` | Format output using Go template |
| `--limit` | Max number of results (default 25) |
| `--no-trunc` | Don't truncate output |

## Steps

1. Execute: `docker search <term>`
2. Display results with stars and official status
3. Suggest `docker pull` for desired image

## Examples

### Basic Search
User: `/docker:search nginx`
→ `docker search nginx`
→ Lists nginx-related images from Docker Hub

### Search with Limit
User: `/docker:search python --limit 10`
→ `docker search python --limit 10`
→ Shows top 10 Python images

### Official Images Only
User: `/docker:search node --filter is-official=true`
→ `docker search node --filter is-official=true`
→ Shows only official Node.js images

### Filter by Stars
User: `/docker:search redis --filter stars=100`
→ `docker search redis --filter stars=100`
→ Shows Redis images with 100+ stars

### Full Description
User: `/docker:search postgres --no-trunc`
→ `docker search postgres --no-trunc`
→ Shows complete descriptions

## Filter Options

| Filter | Description | Example |
|--------|-------------|---------|
| `stars` | Minimum star count | `--filter stars=50` |
| `is-official` | Official images only | `--filter is-official=true` |
| `is-automated` | Automated builds | `--filter is-automated=true` |

## Output Columns

| Column | Description |
|--------|-------------|
| NAME | Image name (repository/image) |
| DESCRIPTION | Brief description |
| STARS | Community star count |
| OFFICIAL | [OK] if official image |
| AUTOMATED | [OK] if automated build |

## Common Searches

### Find Base Images
```bash
# Official language runtimes
docker search python --filter is-official=true
docker search node --filter is-official=true
docker search golang --filter is-official=true

# Official databases
docker search postgres --filter is-official=true
docker search mysql --filter is-official=true
docker search redis --filter is-official=true
```

### Find Popular Images
```bash
# Most starred images for a term
docker search nginx --filter stars=1000

# Popular with full descriptions
docker search alpine --filter stars=500 --no-trunc
```

### Find Specific Variants
```bash
# Search for slim variants
docker search python | grep slim

# Search for alpine variants
docker search node | grep alpine
```

## Custom Format

```bash
# Show only name and stars
docker search nginx --format "{{.Name}}: {{.StarCount}} stars"

# Table format
docker search redis --format "table {{.Name}}\t{{.StarCount}}\t{{.IsOfficial}}"
```

## After Finding an Image

```bash
# Pull the image
docker pull nginx:alpine

# Check available tags (requires Docker Hub or registry API)
# Visit: https://hub.docker.com/_/nginx

# Or use /docker:pull
/docker:pull nginx:alpine
```

## Tips

- Search only queries Docker Hub (not private registries)
- Use `--filter is-official=true` for verified images
- Star count indicates community popularity
- Official images have no namespace prefix (e.g., `nginx` not `user/nginx`)
- Check Docker Hub website for available tags
- Automated builds are from linked GitHub/Bitbucket repos
- Use `--no-trunc` for full descriptions
- Results limited to 25 by default; use `--limit` for more
