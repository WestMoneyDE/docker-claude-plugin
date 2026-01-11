---
name: attach
description: Attach to a running container's stdin/stdout/stderr
allowed-tools:
  - Bash
argument-hint: "<container> [--no-stdin] [--sig-proxy]"
---

# Docker Attach Command

Attach local stdin, stdout, and stderr to a running container.

## Behavior

1. **Identify container**: Find running container by name or ID
2. **Attach streams**: Connect to container's primary process
3. **Interactive**: Send input and receive output
4. **Detach**: Use escape sequence to disconnect

## Options

| Option | Description |
|--------|-------------|
| `--no-stdin` | Do not attach stdin |
| `--sig-proxy` | Proxy signals to process (default: true) |
| `--detach-keys` | Override detach key sequence |

## Steps

1. Verify container is running
2. Execute: `docker attach <container>`
3. Interact with main process
4. Detach with `Ctrl+P, Ctrl+Q`

## Examples

### Basic Attach
User: `/docker:attach api`
→ `docker attach api`
→ Attaches to api container's main process

### Attach Without Stdin
User: `/docker:attach api --no-stdin`
→ `docker attach --no-stdin api`
→ View output only, no input

### Custom Detach Keys
User: `/docker:attach api --detach-keys="ctrl-x"`
→ Use Ctrl+X to detach instead of Ctrl+P,Q

## Detach Sequence

**Default:** `Ctrl+P` then `Ctrl+Q`

This detaches from container without stopping it.

**Warning:** `Ctrl+C` sends SIGINT to the container process and may stop it!

```bash
# Change default detach keys
docker attach --detach-keys="ctrl-d" api
```

## Attach vs Exec

| Feature | `attach` | `exec` |
|---------|----------|--------|
| Target | Main process (PID 1) | New process |
| Multiple clients | Share same view | Independent |
| Container startup | See from beginning | Only new output |
| Exit behavior | May stop container | Just exits shell |
| Use case | Monitor main process | Run commands |

```bash
# Attach to main process
docker attach api

# Run new shell (preferred for interaction)
docker exec -it api /bin/sh
```

## Use Cases

| Scenario | Command |
|----------|---------|
| Monitor startup | `docker attach api` |
| Debug foreground app | `docker attach --sig-proxy=false api` |
| View live logs | `docker attach --no-stdin api` |
| Interactive CLI app | `docker attach cli-tool` |

## Multiple Attachments

Multiple terminals can attach to same container:
- All see the same output
- All can send input (may cause confusion)
- Use `--no-stdin` for additional viewers

```bash
# Terminal 1: Full attach
docker attach api

# Terminal 2: View only
docker attach --no-stdin api
```

## Common Workflows

### Monitor Application Startup
```bash
# Start container
docker run -d --name api myapp

# Immediately attach to see startup logs
docker attach api
# Ctrl+P, Ctrl+Q to detach
```

### Interactive Application
```bash
# Start interactive app
docker run -d -it --name repl python

# Attach to interact
docker attach repl
>>> print("Hello")
Hello
# Ctrl+P, Ctrl+Q to detach
```

## Troubleshooting

| Issue | Cause | Solution |
|-------|-------|----------|
| No output | Process not writing to stdout | Check container logs |
| Ctrl+C stops container | Main process receives SIGINT | Use `--sig-proxy=false` |
| Can't type | stdin not attached | Don't use `--no-stdin` |
| Immediate exit | Container not running | Start container first |

## Tips

- Use `exec` for running new commands
- Use `attach` to see main process output
- Always remember detach sequence: `Ctrl+P, Ctrl+Q`
- `Ctrl+C` may stop the container
- Use `--no-stdin` for safe monitoring
- Better to use `docker logs -f` for just viewing output
