---
name: pull
description: Pull Docker images from registries
allowed-tools:
  - Bash
argument-hint: "<image[:tag]> [--all-tags]"
---

# Docker Pull Command

Pull Docker images from container registries.

## Behavior

1. **Parse image reference**: Identify registry, image name, and tag
2. **Check authentication**: Verify access to private registries
3. **Pull image**: Download from registry
4. **Report**: Show pull progress and final image info

## Image Reference Formats

| Format | Example |
|--------|---------|
| Official | `nginx`, `postgres:15` |
| Docker Hub | `username/image:tag` |
| AWS ECR | `123456789.dkr.ecr.region.amazonaws.com/image:tag` |
| GCR | `gcr.io/project/image:tag` |
| GitHub | `ghcr.io/owner/image:tag` |

## Steps

1. Parse the image reference
2. If no tag specified, default to `latest` (with warning)
3. Pull the image: `docker pull <image>`
4. Show image details after successful pull

## Examples

User: `/docker:pull nginx`
→ `docker pull nginx:latest`
→ Pulls official nginx image

User: `/docker:pull postgres:15-alpine`
→ `docker pull postgres:15-alpine`
→ Pulls specific PostgreSQL version

User: `/docker:pull ghcr.io/owner/app:v1.2.0`
→ Pulls from GitHub Container Registry

User: `/docker:pull node --all-tags`
→ `docker pull --all-tags node`
→ Pulls all node image tags (warning: large download)

## Tips

- Warn when pulling `latest` tag (recommend specific version)
- Show image size after pull
- If authentication fails, show login instructions
- For large images, mention download size
