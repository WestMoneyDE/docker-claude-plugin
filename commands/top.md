---
name: top
description: Display running processes in a container
allowed-tools:
  - Bash
argument-hint: "<container> [ps options]"
---

# Docker Top Command

Display the running processes inside a container.

## Behavior

1. **Identify container**: Find by name or ID
2. **List processes**: Show all processes running in container
3. **Display details**: PID, user, CPU, memory, command
4. **Support ps options**: Pass through to ps command

## Steps

1. Verify container is running
2. Execute: `docker top <container>`
3. Display process table
4. Optionally apply ps format options

## Examples

### Basic Process List
User: `/docker:top api`
→ `docker top api`
→ Shows all processes in api container

Output:
```
UID    PID    PPID   C  STIME  TTY   TIME      CMD
root   1234   1      0  10:00  ?     00:00:05  node /app/index.js
root   1245   1234   0  10:00  ?     00:00:01  /app/worker
```

### With PS Options
User: `/docker:top api -aux`
→ `docker top api -aux`
→ Shows detailed process info

User: `/docker:top api -eo pid,ppid,user,%cpu,%mem,cmd`
→ Custom format with specific columns

### Sort by CPU
User: `/docker:top api -aux --sort=-%cpu`
→ Shows processes sorted by CPU usage

### Sort by Memory
User: `/docker:top api -aux --sort=-%mem`
→ Shows processes sorted by memory usage

## Output Columns

| Column | Description |
|--------|-------------|
| UID | User ID running the process |
| PID | Process ID (host namespace) |
| PPID | Parent process ID |
| C | CPU utilization |
| STIME | Start time |
| TTY | Terminal |
| TIME | Cumulative CPU time |
| CMD | Command with arguments |

## Custom Formats

```bash
# Show only PID and command
docker top <container> -eo pid,cmd

# Show resource usage
docker top <container> -eo pid,user,%cpu,%mem,vsz,rss,cmd

# Show process tree
docker top <container> -ef --forest
```

## Common PS Options

| Option | Description |
|--------|-------------|
| `-a` | All processes |
| `-u` | User-oriented format |
| `-x` | Include processes without TTY |
| `-e` | Every process |
| `-o` | Custom output format |
| `--sort` | Sort by column |

## Use Cases

| Scenario | Command |
|----------|---------|
| Debug hung container | `docker top api` |
| Find memory hogs | `docker top api -aux --sort=-%mem` |
| Check worker processes | `docker top worker` |
| Verify main process | `docker top api -eo pid,cmd` |
| Security audit | `docker top api -eo user,cmd` |

## Top vs Exec ps

| Method | Pros | Cons |
|--------|------|------|
| `docker top` | No ps needed in image | Limited options |
| `docker exec ps` | Full ps features | Requires ps in image |

```bash
# docker top (always works)
docker top api

# exec ps (requires ps in image)
docker exec api ps aux
```

## Tips

- Works only on running containers
- PIDs shown are from host namespace
- PID 1 in container is usually main process
- Multiple processes may indicate forking
- Use with `/docker:stats` for resource monitoring
- Check if container should be single-process
