---
name: Docker Debugging
description: This skill should be used when the user asks to "debug a container", "fix container crash", "container not starting", "container exited", "view container logs", "why is my container failing", "docker troubleshooting", "container won't run", "debug Docker", or mentions container exit codes, container logs, or container health issues.
version: 1.0.0
---

# Docker Debugging

Systematic approaches for diagnosing and resolving Docker container issues in local development and production environments.

## Diagnostic Workflow

When a container fails, follow this systematic approach:

1. **Check container status** - Identify if container is running, exited, or restarting
2. **Examine logs** - Look for error messages and stack traces
3. **Inspect exit codes** - Understand why the process terminated
4. **Check resources** - Verify memory, CPU, and disk constraints
5. **Test connectivity** - Validate network and port bindings
6. **Inspect filesystem** - Check file permissions and mounts

## Container Status Commands

### List All Containers

```bash
# Running containers
docker ps

# All containers including stopped
docker ps -a

# Show only container IDs
docker ps -aq

# Filter by status
docker ps -f "status=exited"
docker ps -f "status=restarting"
```

### Detailed Container Inspection

```bash
# Full container details
docker inspect <container>

# Specific fields
docker inspect --format='{{.State.Status}}' <container>
docker inspect --format='{{.State.ExitCode}}' <container>
docker inspect --format='{{.State.Error}}' <container>
docker inspect --format='{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' <container>
```

## Log Analysis

### Viewing Logs

```bash
# All logs
docker logs <container>

# Follow logs in real-time
docker logs -f <container>

# Last N lines
docker logs --tail 100 <container>

# Logs since timestamp
docker logs --since "2024-01-01T00:00:00" <container>

# Logs with timestamps
docker logs -t <container>
```

### Log Patterns to Look For

| Pattern | Likely Cause |
|---------|--------------|
| `ECONNREFUSED` | Service not ready, wrong port |
| `ENOENT` | Missing file or directory |
| `EACCES` | Permission denied |
| `OOMKilled` | Out of memory |
| `exec format error` | Wrong architecture |
| `no such file or directory` | Missing binary/entrypoint |

## Exit Codes

Exit codes reveal why a container stopped:

| Code | Meaning | Common Cause |
|------|---------|--------------|
| 0 | Success | Normal completion |
| 1 | General error | Application error |
| 126 | Permission error | Cannot execute command |
| 127 | Command not found | Missing binary |
| 137 | SIGKILL (128+9) | OOM killed or `docker kill` |
| 139 | SIGSEGV (128+11) | Segmentation fault |
| 143 | SIGTERM (128+15) | Graceful shutdown |
| 255 | Exit status out of range | Error in script |

### Check Exit Code

```bash
docker inspect --format='{{.State.ExitCode}}' <container>
```

## Interactive Debugging

### Execute Commands in Running Container

```bash
# Open shell
docker exec -it <container> /bin/sh
docker exec -it <container> /bin/bash

# Run specific command
docker exec <container> cat /etc/hosts
docker exec <container> env
docker exec <container> ps aux
```

### Debug Stopped Container

Override entrypoint to get shell access:

```bash
# Start with shell instead of normal entrypoint
docker run -it --entrypoint /bin/sh <image>

# For containers that exited
docker commit <container> debug-image
docker run -it --entrypoint /bin/sh debug-image
```

## Resource Issues

### Memory

```bash
# Check memory usage
docker stats <container>

# Check if OOM killed
docker inspect --format='{{.State.OOMKilled}}' <container>

# Run with memory limit
docker run -m 512m <image>
```

### Disk Space

```bash
# Check disk usage
docker system df

# Clean up unused resources
docker system prune -a

# Check container filesystem usage
docker exec <container> df -h
```

## Network Debugging

### Port Issues

```bash
# Check port bindings
docker port <container>

# Inspect network settings
docker inspect --format='{{json .NetworkSettings.Ports}}' <container>

# Test port from host
curl localhost:8080
nc -zv localhost 8080
```

### DNS and Connectivity

```bash
# Check DNS resolution inside container
docker exec <container> nslookup google.com

# Test connectivity
docker exec <container> ping -c 3 google.com
docker exec <container> wget -q -O- http://service:8080/health
```

### Network Inspection

```bash
# List networks
docker network ls

# Inspect network
docker network inspect bridge

# Check container network
docker inspect --format='{{json .NetworkSettings.Networks}}' <container>
```

## Volume and Mount Issues

### Check Mounts

```bash
# List mounts
docker inspect --format='{{json .Mounts}}' <container>

# Verify volume exists
docker volume ls
docker volume inspect <volume>
```

### Common Mount Problems

| Issue | Solution |
|-------|----------|
| Permission denied | Check file ownership, use `--user` flag |
| File not found | Verify host path exists |
| Empty directory | Ensure mount path is correct |
| Read-only error | Remove `:ro` or fix permissions |

### Fix Permissions

```bash
# Run as specific user
docker run --user $(id -u):$(id -g) <image>

# Fix ownership in Dockerfile
RUN chown -R appuser:appuser /app
```

## Common Problems and Solutions

### Container Keeps Restarting

```bash
# Check restart policy
docker inspect --format='{{.HostConfig.RestartPolicy.Name}}' <container>

# Remove restart policy to debug
docker update --restart=no <container>

# Check logs for crash reason
docker logs --tail 50 <container>
```

### Container Exits Immediately

Possible causes:
1. **No foreground process** - CMD/ENTRYPOINT exits
2. **Missing dependencies** - Required files not present
3. **Configuration error** - Invalid config file

Debug steps:
```bash
# Override command to keep container running
docker run -it <image> /bin/sh -c "while true; do sleep 1000; done"

# Check what CMD is set to
docker inspect --format='{{.Config.Cmd}}' <image>
```

### "exec format error"

Architecture mismatch between image and host.

```bash
# Check image architecture
docker inspect --format='{{.Architecture}}' <image>

# Build for correct platform
docker build --platform linux/amd64 -t myimage .

# Run with platform flag
docker run --platform linux/amd64 <image>
```

### Health Check Failing

```bash
# Check health status
docker inspect --format='{{json .State.Health}}' <container>

# View health check logs
docker inspect --format='{{range .State.Health.Log}}{{.Output}}{{end}}' <container>

# Test health endpoint manually
docker exec <container> wget -q -O- http://localhost:8080/health
```

## Docker Compose Debugging

```bash
# View all service logs
docker compose logs

# Follow specific service
docker compose logs -f <service>

# Check service status
docker compose ps

# Rebuild and restart
docker compose up --build --force-recreate

# View config after variable substitution
docker compose config
```

## Debug Checklist

When a container fails:

- [ ] Check container status: `docker ps -a`
- [ ] Read logs: `docker logs <container>`
- [ ] Check exit code: `docker inspect --format='{{.State.ExitCode}}' <container>`
- [ ] Verify OOM: `docker inspect --format='{{.State.OOMKilled}}' <container>`
- [ ] Test ports: `docker port <container>`
- [ ] Check mounts: `docker inspect --format='{{json .Mounts}}' <container>`
- [ ] Try interactive: `docker run -it --entrypoint /bin/sh <image>`

## Quick Commands Reference

| Task | Command |
|------|---------|
| Status | `docker ps -a` |
| Logs | `docker logs -f <container>` |
| Shell | `docker exec -it <container> /bin/sh` |
| Inspect | `docker inspect <container>` |
| Stats | `docker stats <container>` |
| Exit code | `docker inspect --format='{{.State.ExitCode}}' <container>` |
| Ports | `docker port <container>` |
