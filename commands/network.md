---
name: network
description: Manage Docker networks
allowed-tools:
  - Bash
argument-hint: "<ls|create|rm|inspect|connect|disconnect> [name] [options]"
---

# Docker Network Command

Manage Docker networks for container communication.

## Operations

| Operation | Description |
|-----------|-------------|
| `ls` | List networks |
| `create` | Create a new network |
| `rm` | Remove a network |
| `inspect` | Show network details |
| `connect` | Connect container to network |
| `disconnect` | Disconnect container from network |

## Steps

1. Parse the operation and arguments
2. Execute appropriate docker network command
3. Show results and relevant information

## Examples

### List Networks
User: `/docker:network ls`
→ `docker network ls`
→ Shows all networks

### Create Network
User: `/docker:network create mynet`
→ `docker network create mynet`
→ Creates bridge network

User: `/docker:network create mynet --driver overlay`
→ Creates overlay network for swarm

### Remove Network
User: `/docker:network rm mynet`
→ `docker network rm mynet`
→ Removes the network

### Inspect Network
User: `/docker:network inspect bridge`
→ Shows network details, connected containers, IP range

### Connect/Disconnect
User: `/docker:network connect mynet api`
→ Connects api container to mynet

User: `/docker:network disconnect mynet api`
→ Disconnects api container from mynet

## Network Drivers

| Driver | Use Case |
|--------|----------|
| `bridge` | Default, single host communication |
| `host` | Use host's network stack |
| `none` | No networking |
| `overlay` | Multi-host swarm communication |
| `macvlan` | Assign MAC address to container |

## Common Tasks

| Task | Command |
|------|---------|
| List networks | `docker network ls` |
| Create isolated network | `docker network create mynet` |
| Run container on network | `docker run --network mynet ...` |
| Find container IP | `docker network inspect bridge` |
| Clean unused networks | `docker network prune` |

## Tips

- Containers on same network can communicate by name
- Use custom networks for service isolation
- Default bridge doesn't support DNS resolution between containers
- Use `/docker:inspect` to see container's network settings
