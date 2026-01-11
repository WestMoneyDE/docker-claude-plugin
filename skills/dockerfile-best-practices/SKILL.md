---
name: Dockerfile Best Practices
description: This skill should be used when the user asks to "write a Dockerfile", "create a Dockerfile", "optimize my Dockerfile", "make my Docker image smaller", "secure my Dockerfile", "fix my Dockerfile", "review my Dockerfile", or mentions Dockerfile multi-stage builds, layer caching, or image optimization.
version: 1.0.0
---

# Dockerfile Best Practices

Guidance for writing optimized, secure, and maintainable Dockerfiles for production and local development.

## Core Principles

### 1. Use Specific Base Image Tags

Never use `latest` tag in production. Pin to specific versions for reproducibility.

```dockerfile
# Bad
FROM node:latest

# Good
FROM node:20.11-alpine3.19
```

### 2. Minimize Layers

Combine related commands with `&&` to reduce layer count and image size.

```dockerfile
# Bad - 3 layers
RUN apt-get update
RUN apt-get install -y curl
RUN apt-get clean

# Good - 1 layer
RUN apt-get update && \
    apt-get install -y --no-install-recommends curl && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*
```

### 3. Order Instructions by Change Frequency

Place rarely-changing instructions first to maximize layer caching.

```dockerfile
# Dependencies change less often than source code
COPY package*.json ./
RUN npm ci --only=production

# Source code changes frequently - copy last
COPY . .
```

### 4. Use Multi-Stage Builds

Separate build dependencies from runtime to reduce final image size.

```dockerfile
# Build stage
FROM node:20-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

# Production stage
FROM node:20-alpine AS production
WORKDIR /app
COPY --from=builder /app/dist ./dist
COPY --from=builder /app/node_modules ./node_modules
CMD ["node", "dist/index.js"]
```

### 5. Use .dockerignore

Exclude unnecessary files to speed up builds and reduce context size.

```
# .dockerignore
node_modules
.git
.gitignore
*.md
.env*
Dockerfile*
docker-compose*
.dockerignore
coverage
.nyc_output
```

## Security Best Practices

### Run as Non-Root User

```dockerfile
# Create non-root user
RUN addgroup -g 1001 appgroup && \
    adduser -u 1001 -G appgroup -D appuser

# Set ownership
COPY --chown=appuser:appgroup . .

# Switch to non-root user
USER appuser
```

### Use COPY Instead of ADD

`COPY` is explicit and safer. Use `ADD` only for tar extraction or remote URLs.

```dockerfile
# Prefer COPY
COPY ./src /app/src

# Use ADD only when needed
ADD https://example.com/file.tar.gz /tmp/
```

### Don't Store Secrets in Images

Use build arguments or runtime environment variables.

```dockerfile
# Bad - secret in image history
ENV API_KEY=secret123

# Good - pass at runtime
# docker run -e API_KEY=secret123 myapp
```

### Scan for Vulnerabilities

Use minimal base images and regularly scan for CVEs.

```dockerfile
# Prefer alpine or distroless
FROM node:20-alpine

# Or use distroless for even smaller attack surface
FROM gcr.io/distroless/nodejs20-debian12
```

## Optimization Techniques

### Use Alpine Base Images

Alpine images are ~5MB vs ~100MB+ for full images.

```dockerfile
# Full image: ~950MB
FROM node:20

# Alpine: ~140MB
FROM node:20-alpine
```

### Clean Up in Same Layer

Remove caches and temp files in the same RUN instruction.

```dockerfile
RUN apt-get update && \
    apt-get install -y --no-install-recommends package && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
```

### Use --no-install-recommends

Skip optional packages when installing with apt.

```dockerfile
RUN apt-get install -y --no-install-recommends curl wget
```

### Leverage Build Cache

Structure commands to maximize cache hits.

```dockerfile
# Cache dependencies separately from code
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .
```

## Common Patterns

### Node.js Application

```dockerfile
FROM node:20-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

FROM node:20-alpine
WORKDIR /app
ENV NODE_ENV=production
RUN addgroup -g 1001 nodejs && adduser -u 1001 -G nodejs -D nodejs
COPY --from=builder --chown=nodejs:nodejs /app/dist ./dist
COPY --from=builder --chown=nodejs:nodejs /app/node_modules ./node_modules
USER nodejs
EXPOSE 3000
CMD ["node", "dist/index.js"]
```

### Python Application

```dockerfile
FROM python:3.12-slim AS builder
WORKDIR /app
RUN pip install --no-cache-dir poetry
COPY pyproject.toml poetry.lock ./
RUN poetry export -f requirements.txt -o requirements.txt

FROM python:3.12-slim
WORKDIR /app
RUN useradd -m -u 1001 appuser
COPY --from=builder /app/requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY --chown=appuser:appuser . .
USER appuser
CMD ["python", "main.py"]
```

### Go Application

```dockerfile
FROM golang:1.22-alpine AS builder
WORKDIR /app
COPY go.mod go.sum ./
RUN go mod download
COPY . .
RUN CGO_ENABLED=0 GOOS=linux go build -ldflags="-s -w" -o /app/main

FROM scratch
COPY --from=builder /app/main /main
ENTRYPOINT ["/main"]
```

## Health Checks

Add HEALTHCHECK for container orchestration.

```dockerfile
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
  CMD wget --quiet --tries=1 --spider http://localhost:3000/health || exit 1
```

## Labels and Metadata

Add labels for documentation and automation.

```dockerfile
LABEL org.opencontainers.image.title="My App"
LABEL org.opencontainers.image.version="1.0.0"
LABEL org.opencontainers.image.description="Application description"
LABEL org.opencontainers.image.source="https://github.com/user/repo"
```

## Quick Reference

| Practice | Impact |
|----------|--------|
| Alpine base images | 80-90% size reduction |
| Multi-stage builds | 50-70% size reduction |
| Layer consolidation | 10-30% size reduction |
| .dockerignore | Faster builds |
| Non-root user | Security hardening |
| Specific tags | Reproducibility |
| HEALTHCHECK | Better orchestration |

## Additional Resources

Consult `references/` for detailed examples and advanced patterns when needed.
