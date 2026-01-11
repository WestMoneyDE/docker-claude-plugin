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
| `/docker:run` | Run containers | `/docker:run nginx --port 8080:80` |
| `/docker:compose` | Compose operations | `/docker:compose up` |
| `/docker:logs` | View container logs | `/docker:logs api --tail 50` |
| `/docker:ps` | List containers | `/docker:ps --all` |
| `/docker:push` | Push to registries | `/docker:push myapp:v1 ghcr.io/user` |
| `/docker:clean` | Remove unused resources | `/docker:clean --all` |

### Command Details

#### `/docker:build [name:tag] [options]`
Build Docker images from a Dockerfile.
- Auto-detects Dockerfile in current directory
- Supports `--no-cache` for fresh builds
- Shows build progress and final image size

#### `/docker:run <image> [options]`
Run containers with common options.
- `-p host:container` - Port mapping
- `-e KEY=VALUE` - Environment variables
- `--name` - Container name
- Defaults to detached mode with auto-cleanup

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
