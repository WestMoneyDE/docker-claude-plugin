---
name: context
description: Manage Docker contexts for multi-environment deployments
allowed-tools:
  - Bash
argument-hint: "<ls|create|use|inspect|rm|export|import> [name] [options]"
---

# Docker Context Command

Manage Docker contexts to switch between different Docker environments (local, remote servers, cloud).

## Operations

| Operation | Description |
|-----------|-------------|
| `ls` | List available contexts |
| `create` | Create a new context |
| `use` | Switch to a context |
| `inspect` | Show context details |
| `rm` | Remove a context |
| `export` | Export context to file |
| `import` | Import context from file |

## Steps

1. Identify operation and arguments
2. Execute appropriate docker context command
3. Report current context status

## Examples

### List Contexts
User: `/docker:context ls`
→ `docker context ls`
→ Shows all contexts with current one marked

Output:
```
NAME        DESCRIPTION                   DOCKER ENDPOINT
default *   Current DOCKER_HOST           unix:///var/run/docker.sock
prod        Production server             ssh://user@prod.example.com
staging     Staging environment           ssh://user@staging.example.com
```

### Create Context
User: `/docker:context create prod --docker "host=ssh://user@prod.example.com"`
→ Creates context for remote production server

User: `/docker:context create staging --docker "host=tcp://staging.example.com:2376,ca=~/.docker/ca.pem,cert=~/.docker/cert.pem,key=~/.docker/key.pem"`
→ Creates context with TLS authentication

### Switch Context
User: `/docker:context use prod`
→ `docker context use prod`
→ Switches to production environment

User: `/docker:context use default`
→ Switches back to local Docker

### Inspect Context
User: `/docker:context inspect prod`
→ Shows full configuration of prod context

### Remove Context
User: `/docker:context rm staging`
→ `docker context rm staging`
→ Removes staging context

### Export/Import
User: `/docker:context export prod > prod-context.tar`
→ Exports context for sharing

User: `/docker:context import prod-backup prod-context.tar`
→ Imports context from file

## Context Types

### Local Docker
```bash
# Default context - local Docker daemon
docker context create local --docker "host=unix:///var/run/docker.sock"
```

### SSH Remote
```bash
# Connect to remote Docker via SSH
docker context create remote --docker "host=ssh://user@server.example.com"
```

### TCP with TLS
```bash
# Secure TCP connection
docker context create secure --docker "host=tcp://server:2376,ca=/path/ca.pem,cert=/path/cert.pem,key=/path/key.pem"
```

### Cloud Contexts

#### AWS ECS
```bash
# Requires docker/ecs-plugin
docker context create ecs myecs --from-env
```

#### Azure ACI
```bash
# Requires docker/aci-plugin
docker context create aci myaci
```

## Multi-Environment Workflow

### Setup Environments
```bash
# Create contexts for each environment
docker context create dev --docker "host=unix:///var/run/docker.sock"
docker context create staging --docker "host=ssh://deploy@staging.example.com"
docker context create prod --docker "host=ssh://deploy@prod.example.com"
```

### Deploy to Environment
```bash
# Deploy to staging
docker context use staging
docker compose up -d

# Deploy to production
docker context use prod
docker compose up -d

# Return to local
docker context use default
```

### Quick Commands with Context
```bash
# Run command in specific context without switching
docker --context prod ps
docker --context staging logs api
```

## Environment Variables

| Variable | Description |
|----------|-------------|
| `DOCKER_CONTEXT` | Override current context |
| `DOCKER_HOST` | Override Docker endpoint |

```bash
# Temporary context override
DOCKER_CONTEXT=prod docker ps

# Or use --context flag
docker --context prod ps
```

## SSH Context Setup

### Prerequisites
```bash
# Ensure SSH access to remote host
ssh user@server.example.com

# Docker must be installed on remote
ssh user@server.example.com docker version
```

### Create SSH Context
```bash
# Basic SSH context
docker context create myserver --docker "host=ssh://user@server.example.com"

# With custom SSH options
docker context create myserver --docker "host=ssh://user@server.example.com" \
  --description "Production server"
```

### SSH Key Authentication
```bash
# Use specific SSH key
docker context create myserver \
  --docker "host=ssh://user@server.example.com" \
  --default-stack-orchestrator swarm

# SSH config (~/.ssh/config) is respected
Host server.example.com
  User deploy
  IdentityFile ~/.ssh/deploy_key
```

## Common Patterns

| Scenario | Command |
|----------|---------|
| List all contexts | `docker context ls` |
| Show current context | `docker context show` |
| Switch environment | `docker context use <name>` |
| Run in specific context | `docker --context <name> <command>` |
| Check remote status | `docker --context prod info` |
| Deploy to remote | `docker --context prod compose up -d` |

## Tips

- Current context is marked with `*` in `ls` output
- Use `docker context show` to see active context
- SSH contexts require Docker on remote host
- Use `--context` flag for one-off commands
- Export contexts to share with team members
- Contexts persist across terminal sessions
- Use descriptive names: `prod`, `staging`, `dev-local`
