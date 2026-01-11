---
name: kill
description: Kill running containers immediately with a signal
allowed-tools:
  - Bash
argument-hint: "<container> [--signal SIGKILL]"
---

# Docker Kill Command

Send a signal to running containers to stop them immediately.

## Behavior

1. **Identify container**: Find by name or ID
2. **Send signal**: Default SIGKILL or specified signal
3. **Terminate**: Container stops immediately
4. **Report**: Confirm container was killed

## Options

| Option | Description |
|--------|-------------|
| `--signal, -s` | Signal to send (default: SIGKILL) |

## Common Signals

| Signal | Number | Effect |
|--------|--------|--------|
| `SIGKILL` | 9 | Force kill (cannot be caught) |
| `SIGTERM` | 15 | Graceful termination request |
| `SIGHUP` | 1 | Reload configuration |
| `SIGUSR1` | 10 | User-defined signal 1 |
| `SIGUSR2` | 12 | User-defined signal 2 |

## Steps

1. Verify container is running
2. Send signal: `docker kill <container>`
3. If signal specified, use `-s` flag
4. Confirm container stopped

## Examples

User: `/docker:kill api`
→ `docker kill api`
→ Immediately kills api container (SIGKILL)

User: `/docker:kill api --signal SIGTERM`
→ `docker kill -s SIGTERM api`
→ Sends graceful termination signal

User: `/docker:kill nginx --signal SIGHUP`
→ `docker kill -s SIGHUP nginx`
→ Reloads nginx configuration

User: `/docker:kill $(docker ps -q)`
→ Kills all running containers

## Kill vs Stop

| Command | Signal | Behavior |
|---------|--------|----------|
| `docker stop` | SIGTERM, then SIGKILL | Graceful shutdown with timeout |
| `docker kill` | SIGKILL (default) | Immediate termination |

## When to Use Kill

| Scenario | Recommendation |
|----------|----------------|
| Unresponsive container | Use `kill` |
| Normal shutdown | Use `stop` |
| Reload config (nginx) | Use `kill -s SIGHUP` |
| Frozen process | Use `kill` |
| Graceful shutdown | Use `stop` or `kill -s SIGTERM` |

## Tips

- Use `stop` for graceful shutdown in most cases
- `kill` is instant - no cleanup time for container
- Some apps handle SIGHUP to reload config
- SIGKILL cannot be caught or ignored
- Use `/docker:ps` to verify container stopped
