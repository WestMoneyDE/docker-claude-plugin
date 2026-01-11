# Docker Plugin for Claude Code

A comprehensive Docker container management plugin focused on local development. Build, run, debug, and optimize containers with AI-powered assistance.

## Features

- **Build & Run**: Build images and run containers with smart defaults
- **Docker Compose**: Manage multi-container applications
- **Debugging**: Diagnose container issues with AI-powered analysis
- **Optimization**: Get recommendations for smaller, faster, more secure images

## Installation

```bash
# Run Claude Code with the plugin
claude --plugin-dir /home/administrator/docker

# Or copy to your plugins directory
cp -r /home/administrator/docker ~/.claude/plugins/
```

## Requirements

- Docker installed and running
- Docker Compose (for compose commands)

## Commands

| Command | Description | Example |
|---------|-------------|---------|
| `/docker:build` | Build Docker images | `/docker:build myapp:v1` |
| `/docker:buildx` | Multi-platform builds | `/docker:buildx build --platform linux/amd64,linux/arm64` |
| `/docker:run` | Run containers | `/docker:run nginx --port 8080:80` |
| `/docker:create` | Create container without starting | `/docker:create --name api nginx` |
| `/docker:start` | Start stopped containers | `/docker:start api` |
| `/docker:attach` | Attach to running container | `/docker:attach api` |
| `/docker:commit` | Create image from container changes | `/docker:commit api myapp:v2` |
| `/docker:export` | Export container filesystem to tar | `/docker:export api -o api.tar` |
| `/docker:stop` | Stop running containers | `/docker:stop api` |
| `/docker:pause` | Pause container processes | `/docker:pause api` |
| `/docker:unpause` | Unpause paused containers | `/docker:unpause api` |
| `/docker:restart` | Restart containers | `/docker:restart api` |
| `/docker:rm` | Remove stopped containers | `/docker:rm api` |
| `/docker:rename` | Rename a container | `/docker:rename old-name new-name` |
| `/docker:update` | Update container resources | `/docker:update api --memory 1g` |
| `/docker:exec` | Execute commands in container | `/docker:exec api /bin/sh` |
| `/docker:pull` | Pull images from registries | `/docker:pull nginx:alpine` |
| `/docker:images` | List local images | `/docker:images` |
| `/docker:save` | Save images to tar archive | `/docker:save myapp -o myapp.tar` |
| `/docker:load` | Load images from tar archive | `/docker:load -i myapp.tar` |
| `/docker:inspect` | Show container/image details | `/docker:inspect api` |
| `/docker:compose` | Compose operations | `/docker:compose up` |
| `/docker:logs` | View container logs | `/docker:logs api --tail 50` |
| `/docker:ps` | List containers | `/docker:ps --all` |
| `/docker:network` | Manage Docker networks | `/docker:network ls` |
| `/docker:volume` | Manage Docker volumes | `/docker:volume ls` |
| `/docker:stats` | Live resource usage stats | `/docker:stats api` |
| `/docker:top` | Display container processes | `/docker:top api` |
| `/docker:port` | List port mappings | `/docker:port api` |
| `/docker:system` | System info and disk usage | `/docker:system df` |
| `/docker:context` | Manage multi-environment contexts | `/docker:context use prod` |
| `/docker:tag` | Tag images | `/docker:tag myapp:latest myapp:v1` |
| `/docker:cp` | Copy files to/from containers | `/docker:cp api:/logs ./` |
| `/docker:history` | Show image layer history | `/docker:history nginx` |
| `/docker:diff` | Show container filesystem changes | `/docker:diff api` |
| `/docker:kill` | Kill containers immediately | `/docker:kill api` |
| `/docker:wait` | Wait for containers to stop | `/docker:wait job` |
| `/docker:login` | Log in to a registry | `/docker:login ghcr.io` |
| `/docker:logout` | Log out from a registry | `/docker:logout` |
| `/docker:push` | Push to registries | `/docker:push myapp:v1 ghcr.io/user` |
| `/docker:clean` | Remove unused resources | `/docker:clean --all` |

### Command Details

#### `/docker:build [name:tag] [options]`
Build Docker images from a Dockerfile.
- Auto-detects Dockerfile in current directory
- Supports `--no-cache` for fresh builds
- Shows build progress and final image size

#### `/docker:buildx <operation> [options]`
Build multi-platform images with BuildKit.
- `--platform linux/amd64,linux/arm64` - Target platforms
- `--push` - Push to registry after build
- `--load` - Load locally (single platform)

#### `/docker:run <image> [options]`
Run containers with common options.
- `-p host:container` - Port mapping
- `-e KEY=VALUE` - Environment variables
- `--name` - Container name
- Defaults to detached mode with auto-cleanup

#### `/docker:create <image> [options]`
Create a container without starting it.
- Same options as `docker run`
- Useful for pre-configuration

#### `/docker:attach <container>`
Attach to a running container's streams.
- Detach with `Ctrl+P, Ctrl+Q`
- `--no-stdin` for view-only

#### `/docker:commit <container> [repository[:tag]]`
Create a new image from a container's changes.
- `--change` - Apply Dockerfile instruction to image
- `--message` - Commit message
- Great for saving manual configuration or debugging

#### `/docker:export <container> -o <file.tar>`
Export a container's filesystem as a tar archive.
- Creates flat filesystem (no image layers)
- Use `docker import` to create image from archive
- Useful for backups, migrations, security audits

#### `/docker:start <container>`
Start stopped containers.
- `--attach` - Attach to output
- `--interactive` - Interactive mode

#### `/docker:rm <container>`
Remove stopped containers.
- `--force` - Force remove running containers
- `--volumes` - Also remove anonymous volumes

#### `/docker:rename <old> <new>`
Rename a container.
- Works on running and stopped containers
- Must be unique name

#### `/docker:update <container> [options]`
Update container resource limits.
- `--memory` - Memory limit (512m, 1g)
- `--cpus` - CPU limit (0.5, 2)
- `--restart` - Restart policy

#### `/docker:exec <container> [command]`
Execute commands inside running containers.
- Opens interactive shell by default
- Supports any command execution
- Use for debugging and inspection

#### `/docker:stop <container> [options]`
Stop running containers gracefully.
- `--all` - Stop all running containers
- `--time` - Seconds to wait before force kill

#### `/docker:pause <container>`
Pause all processes in a container.
- Freezes without terminating
- Memory and state preserved

#### `/docker:unpause <container>`
Resume paused containers.
- Instant resumption
- Processes continue from where paused

#### `/docker:restart <container>`
Restart running containers.
- Preserves container configuration
- `--time` - Seconds to wait for graceful stop

#### `/docker:pull <image[:tag]>`
Pull images from container registries.
- Supports Docker Hub, ECR, GCR, GitHub
- Warns when pulling `latest` tag

#### `/docker:images [repository]`
List Docker images on the system.
- `--all` - Show intermediate layers
- `--filter dangling=true` - Show untagged images

#### `/docker:save <image> -o <file.tar>`
Save images to a tar archive.
- Offline transfer and backup
- Supports multiple images

#### `/docker:load -i <file.tar>`
Load images from a tar archive.
- Restore backups and offline installs
- Works with compressed archives

#### `/docker:inspect <container|image>`
Display detailed information about containers or images.
- Shows state, network, mounts, config
- `--format` for specific fields (ip, ports, env)

#### `/docker:compose <operation> [service]`
Manage Docker Compose applications.
- `up` - Start services (detached)
- `down` - Stop and remove
- `restart` - Restart services
- `logs` - Follow service logs
- `ps` - List services

#### `/docker:logs <container> [options]`
View container logs.
- `--tail N` - Last N lines
- `--follow` - Stream in real-time
- `--since` - Logs since timestamp

#### `/docker:ps [options]`
List Docker containers.
- `--all` - Include stopped containers
- `--filter` - Filter by status

#### `/docker:network <operation> [name]`
Manage Docker networks.
- `ls` - List networks
- `create` - Create network
- `rm` - Remove network
- `connect/disconnect` - Attach containers

#### `/docker:volume <operation> [name]`
Manage Docker volumes for persistent data.
- `ls` - List volumes
- `create` - Create volume
- `rm` - Remove volume
- `prune` - Remove unused volumes

#### `/docker:stats [container]`
Display live resource usage statistics.
- CPU, memory, network, disk I/O
- `--no-stream` - Single snapshot

#### `/docker:top <container>`
Display running processes in a container.
- Shows PID, user, CPU, memory, command
- Supports ps options for custom formats

#### `/docker:port <container>`
List port mappings for a container.
- Shows host:container port bindings
- Query specific port with protocol

#### `/docker:system <operation>`
Manage Docker system resources.
- `df` - Show disk usage
- `prune` - Remove unused data
- `info` - System information

#### `/docker:context <operation> [name]`
Manage Docker contexts for multi-environment.
- `ls` - List contexts
- `create` - Create context (SSH, TCP)
- `use` - Switch environment

#### `/docker:tag <source> <target>`
Create a new tag for an image.
- Prepare images for registry push
- Version tagging for releases

#### `/docker:cp <src> <dest>`
Copy files between container and host.
- `container:/path` for container paths
- Works with running or stopped containers

#### `/docker:history <image>`
Show image layer history.
- See Dockerfile commands that created layers
- `--no-trunc` - Show full commands

#### `/docker:diff <container>`
Show container filesystem changes.
- `A` - Added, `C` - Changed, `D` - Deleted
- Useful for debugging and auditing

#### `/docker:kill <container> [--signal]`
Kill containers immediately with a signal.
- Default: SIGKILL (immediate termination)
- `--signal SIGHUP` - Reload config (nginx)

#### `/docker:wait <container>`
Wait for containers to stop, return exit codes.
- Blocks until container exits
- Useful in CI/CD scripts

#### `/docker:login [registry]`
Log in to a Docker registry.
- Docker Hub, GitHub, AWS ECR, GCR
- `--password-stdin` for secure auth

#### `/docker:logout [registry]`
Log out from a Docker registry.
- Removes stored credentials
- Default: Docker Hub

#### `/docker:push <image:tag> [registry]`
Push images to container registries.
- Supports Docker Hub, ECR, GCR, GitHub
- Shows authentication instructions if needed

#### `/docker:clean [options]`
Clean up unused Docker resources.
- `--all` - Remove all unused images
- `--volumes` - Also remove volumes
- Shows disk space reclaimed

## Agents

### Dockerfile Optimizer
Automatically activates when you create or edit a Dockerfile. Analyzes your Dockerfile and suggests improvements for:
- Image size reduction
- Build speed optimization
- Security hardening
- Best practices compliance

### Container Debugger
Activates when containers fail or crash. Systematically diagnoses:
- Exit codes and their meanings
- Log analysis for errors
- Resource issues (OOM, CPU)
- Network and connectivity problems
- Volume and permission issues

## Skills

### Dockerfile Best Practices
Triggered when you ask about writing or optimizing Dockerfiles. Provides guidance on:
- Multi-stage builds
- Layer optimization
- Security best practices
- Common patterns for Node.js, Python, Go

### Docker Debugging
Triggered when troubleshooting container issues. Covers:
- Systematic diagnostic workflow
- Exit code analysis
- Log patterns and meanings
- Common problems and solutions

## Example Usage

```
# Build an image
> /docker:build myapp:latest

# Run with port mapping
> /docker:run myapp --port 3000:3000

# Check running containers
> /docker:ps

# View logs
> /docker:logs myapp

# Start compose services
> /docker:compose up

# Clean up
> /docker:clean --all
```

## Troubleshooting

**Plugin not loading?**
- Ensure Docker is installed: `docker --version`
- Check plugin path is correct
- Restart Claude Code after adding plugin

**Commands not found?**
- Commands are prefixed with `/docker:`
- Use `/help` to see available commands

## License

MIT
