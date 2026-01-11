---
name: buildx
description: Build multi-platform Docker images with BuildKit
allowed-tools:
  - Bash
  - Read
  - Glob
argument-hint: "<build|ls|create|use|inspect> [options]"
---

# Docker Buildx Command

Build multi-platform Docker images using BuildKit for cross-architecture support.

## Operations

| Operation | Description |
|-----------|-------------|
| `build` | Build multi-platform image |
| `ls` | List available builders |
| `create` | Create a new builder instance |
| `use` | Switch to a builder |
| `inspect` | Show builder details |
| `rm` | Remove a builder |

## Build Options

| Option | Description |
|--------|-------------|
| `--platform` | Target platforms (linux/amd64,linux/arm64) |
| `--push` | Push to registry after build |
| `--load` | Load into local Docker (single platform only) |
| `--tag, -t` | Image name and tag |
| `--file, -f` | Dockerfile path |
| `--no-cache` | Build without cache |
| `--builder` | Specify builder to use |

## Steps

1. Ensure buildx is available: `docker buildx version`
2. Create/use multi-platform builder if needed
3. Execute buildx command
4. Report build results and platforms

## Examples

### Multi-Platform Build
User: `/docker:buildx build --platform linux/amd64,linux/arm64 -t myapp:latest --push .`
→ Builds for both AMD64 and ARM64, pushes to registry

User: `/docker:buildx build --platform linux/amd64,linux/arm64,linux/arm/v7 -t myapp:v1 --push .`
→ Builds for AMD64, ARM64, and ARMv7

### Setup Builder
User: `/docker:buildx create --name multiplatform --use`
→ Creates and switches to multi-platform builder

User: `/docker:buildx ls`
→ Lists all available builders

### Single Platform with Load
User: `/docker:buildx build --platform linux/amd64 -t myapp:latest --load .`
→ Builds for AMD64 and loads into local Docker

### Inspect Builder
User: `/docker:buildx inspect`
→ Shows current builder configuration and supported platforms

## Common Platforms

| Platform | Architecture | Use Case |
|----------|--------------|----------|
| `linux/amd64` | x86_64 | Standard servers, Intel/AMD |
| `linux/arm64` | ARM 64-bit | Apple Silicon, AWS Graviton |
| `linux/arm/v7` | ARM 32-bit | Raspberry Pi, IoT |
| `linux/arm/v6` | ARM 32-bit | Older Raspberry Pi |
| `linux/386` | x86 32-bit | Legacy systems |
| `linux/ppc64le` | PowerPC | IBM Power |
| `linux/s390x` | IBM Z | Mainframes |

## Builder Setup

### Create Multi-Platform Builder
```bash
# Create builder with docker-container driver
docker buildx create --name mybuilder --driver docker-container --use

# Bootstrap the builder
docker buildx inspect --bootstrap

# Verify platforms
docker buildx ls
```

### Use Existing Builder
```bash
# List builders
docker buildx ls

# Switch builder
docker buildx use mybuilder
```

## Multi-Platform Workflow

### 1. Setup (One-time)
```bash
docker buildx create --name multiarch --use
docker buildx inspect --bootstrap
```

### 2. Build and Push
```bash
docker buildx build \
  --platform linux/amd64,linux/arm64 \
  -t username/myapp:latest \
  --push .
```

### 3. Verify
```bash
docker buildx imagetools inspect username/myapp:latest
```

## Build Strategies

| Strategy | Command | Use Case |
|----------|---------|----------|
| Build + Push | `--push` | CI/CD, release builds |
| Build + Load | `--load` | Local testing (single platform) |
| Build Only | (no flag) | Validation, cache warming |
| Build + Export | `--output type=tar` | Offline transfer |

## CI/CD Example

```yaml
# GitHub Actions
- name: Set up QEMU
  uses: docker/setup-qemu-action@v3

- name: Set up Docker Buildx
  uses: docker/setup-buildx-action@v3

- name: Build and push
  uses: docker/build-push-action@v5
  with:
    platforms: linux/amd64,linux/arm64
    push: true
    tags: user/app:latest
```

## Dockerfile Tips for Multi-Platform

```dockerfile
# Use multi-platform base images
FROM --platform=$BUILDPLATFORM golang:1.22 AS builder

# Access build args for cross-compilation
ARG TARGETPLATFORM
ARG TARGETOS
ARG TARGETARCH

# Cross-compile for target
RUN CGO_ENABLED=0 GOOS=$TARGETOS GOARCH=$TARGETARCH go build -o /app

# Final stage inherits target platform
FROM alpine:3.19
COPY --from=builder /app /app
```

## Tips

- Use `--push` for multi-platform (can't load multiple platforms locally)
- Use `--load` only for single platform local testing
- QEMU enables building non-native architectures
- Native builds are faster than emulated
- Cache is shared across platforms with BuildKit
- Use `docker buildx imagetools` to inspect manifests
