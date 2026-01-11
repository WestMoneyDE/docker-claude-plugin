---
name: history
description: Show the history of a Docker image
allowed-tools:
  - Bash
argument-hint: "<image> [--no-trunc] [--quiet]"
---

# Docker History Command

Show the history and layers of a Docker image.

## Behavior

1. **Identify image**: Find by name or ID
2. **Show layers**: Display each layer's creation command
3. **Show metadata**: Size, creation time, and instructions

## Options

| Option | Description |
|--------|-------------|
| `--no-trunc` | Show full command (not truncated) |
| `--quiet, -q` | Only show layer IDs |
| `--format` | Custom output format |

## Output Columns

| Column | Description |
|--------|-------------|
| IMAGE | Layer ID |
| CREATED | When layer was created |
| CREATED BY | Dockerfile instruction |
| SIZE | Layer size |
| COMMENT | Optional comment |

## Steps

1. Verify image exists locally
2. Execute: `docker history <image>`
3. Format output for readability
4. Optionally show full commands with `--no-trunc`

## Examples

User: `/docker:history nginx:alpine`
→ `docker history nginx:alpine`
→ Shows all layers of nginx:alpine image

User: `/docker:history myapp --no-trunc`
→ `docker history --no-trunc myapp`
→ Shows full Dockerfile commands (not truncated)

User: `/docker:history myapp:v1 --quiet`
→ `docker history -q myapp:v1`
→ Shows only layer IDs

## Use Cases

| Scenario | Benefit |
|----------|---------|
| Debug build | See exact commands that created layers |
| Audit image | Check for suspicious or unwanted commands |
| Optimize size | Find large layers to optimize |
| Understand image | Learn how base images are built |
| Security review | Check for secrets or vulnerabilities |

## Layer Analysis

```
IMAGE          CREATED       CREATED BY                    SIZE
abc123         2 days ago    CMD ["nginx" "-g" "daemon…    0B
def456         2 days ago    EXPOSE 80                     0B
ghi789         2 days ago    COPY nginx.conf /etc/nginx    1.2kB
jkl012         2 days ago    RUN apk add --no-cache nginx  5.6MB
mno345         3 weeks ago   /bin/sh -c #(nop) CMD ["/…    0B
```

## Tips

- Large SIZE values indicate optimization opportunities
- `<missing>` layers come from base images
- Use `--no-trunc` to see full RUN commands
- Compare history of different image versions
- Helps understand third-party images
