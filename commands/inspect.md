---
name: inspect
description: Display detailed information about containers or images
allowed-tools:
  - Bash
argument-hint: "<container|image> [--format field]"
---

# Docker Inspect Command

Display detailed information about Docker containers or images.

## Behavior

1. **Identify target**: Determine if inspecting container or image
2. **Run inspect**: Get full JSON details
3. **Format output**: Show relevant information clearly
4. **Highlight key info**: Surface most useful details

## Common Fields

### Container Inspection
| Field | Description |
|-------|-------------|
| State | Running, exited, status, exit code |
| Config | Env vars, CMD, entrypoint, exposed ports |
| NetworkSettings | IP address, ports, networks |
| Mounts | Volume bindings |
| HostConfig | Resource limits, restart policy |

### Image Inspection
| Field | Description |
|-------|-------------|
| Config | Env, CMD, entrypoint, labels |
| Architecture | amd64, arm64 |
| Size | Image size in bytes |
| Layers | Layer history |

## Steps

1. Determine if target is container or image
2. Run `docker inspect <target>`
3. Format and display key information
4. If specific field requested, extract with `--format`

## Examples

User: `/docker:inspect api`
→ Shows container details (state, network, mounts)

User: `/docker:inspect nginx:alpine`
→ Shows image details (size, config, layers)

User: `/docker:inspect api --format ip`
→ `docker inspect --format='{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' api`
→ Shows only IP address

User: `/docker:inspect api --format env`
→ Shows environment variables

User: `/docker:inspect api --format mounts`
→ Shows volume mounts

## Useful Format Shortcuts

| Shortcut | Extracts |
|----------|----------|
| `ip` | Container IP address |
| `ports` | Port mappings |
| `env` | Environment variables |
| `mounts` | Volume mounts |
| `state` | Running state and exit code |
| `cmd` | Command/entrypoint |

## Tips

- Default to showing summary of most useful fields
- For full JSON output, mention `docker inspect <target>` directly
- Help interpret exit codes and states
- Show related commands for common follow-up actions
