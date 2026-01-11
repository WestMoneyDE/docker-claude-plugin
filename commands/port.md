---
name: port
description: List port mappings for a container
allowed-tools:
  - Bash
argument-hint: "<container> [private_port[/protocol]]"
---

# Docker Port Command

List port mappings or show the public-facing port for a specific container port.

## Behavior

1. **Identify container**: Find by name or ID
2. **List mappings**: Show all port bindings
3. **Query specific**: Show mapping for specific port
4. **Report**: Display host:port bindings

## Steps

1. Verify container exists
2. Execute: `docker port <container>`
3. Display port mappings
4. Show how to access services

## Examples

### List All Port Mappings
User: `/docker:port api`
→ `docker port api`
→ Shows all port mappings

Output:
```
3000/tcp -> 0.0.0.0:3000
3000/tcp -> [::]:3000
8080/tcp -> 0.0.0.0:8080
```

### Query Specific Port
User: `/docker:port api 3000`
→ `docker port api 3000`
→ Shows mapping for port 3000

Output:
```
0.0.0.0:3000
[::]:3000
```

### Query with Protocol
User: `/docker:port api 3000/tcp`
→ `docker port api 3000/tcp`
→ Shows TCP mapping for port 3000

User: `/docker:port api 53/udp`
→ Shows UDP mapping for port 53

## Output Format

```
<container_port>/<protocol> -> <host_ip>:<host_port>
```

| Part | Description |
|------|-------------|
| container_port | Port inside container |
| protocol | tcp or udp |
| host_ip | Bound host IP (0.0.0.0 = all interfaces) |
| host_port | Port on host machine |

## Common Scenarios

### Web Application
```bash
$ docker port webapp
80/tcp -> 0.0.0.0:8080
443/tcp -> 0.0.0.0:8443
```
Access at: `http://localhost:8080`

### Database
```bash
$ docker port postgres
5432/tcp -> 0.0.0.0:5432
```
Connect: `psql -h localhost -p 5432`

### Multiple Services
```bash
$ docker port fullstack
3000/tcp -> 0.0.0.0:3000   # Frontend
4000/tcp -> 0.0.0.0:4000   # API
5432/tcp -> 0.0.0.0:5432   # Database
```

## Port Binding Types

| Binding | Meaning |
|---------|---------|
| `0.0.0.0:8080` | All IPv4 interfaces |
| `[::]:8080` | All IPv6 interfaces |
| `127.0.0.1:8080` | Localhost only |
| `192.168.1.10:8080` | Specific interface |

## No Ports Shown?

If no output, container may have:
- No exposed ports
- Ports not published (`-p` flag missing)
- Using host network mode

```bash
# Check container config
docker inspect --format='{{json .NetworkSettings.Ports}}' <container>

# Check if using host network
docker inspect --format='{{.HostConfig.NetworkMode}}' <container>
```

## Port vs Inspect

```bash
# Quick port check
docker port api

# Detailed port info
docker inspect --format='{{json .NetworkSettings.Ports}}' api

# Exposed but not published
docker inspect --format='{{json .Config.ExposedPorts}}' api
```

## Related Commands

| Task | Command |
|------|---------|
| Run with port | `docker run -p 8080:80 nginx` |
| List ports | `docker port <container>` |
| Check listening | `docker exec api netstat -tlnp` |
| All container ports | `docker ps --format "{{.Names}}: {{.Ports}}"` |

## Tips

- No output means no published ports
- Use `docker ps` to see ports for all containers
- `0.0.0.0` means accessible from any interface
- `127.0.0.1` means localhost only
- Check firewall if port not accessible
- Use `/docker:inspect` for detailed network info
