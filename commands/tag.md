---
name: tag
description: Create a tag for a Docker image
allowed-tools:
  - Bash
argument-hint: "<source-image[:tag]> <target-image[:tag]>"
---

# Docker Tag Command

Create a new tag for an existing Docker image.

## Behavior

1. **Identify source**: Find the source image
2. **Create tag**: Apply new tag to image
3. **Verify**: Show the newly tagged image

## Use Cases

| Scenario | Example |
|----------|---------|
| Add version tag | `myapp:latest` → `myapp:v1.0.0` |
| Prepare for registry | `myapp` → `ghcr.io/user/myapp` |
| Create release | `myapp:dev` → `myapp:stable` |
| Multi-arch tag | `myapp:amd64` → `myapp:latest` |

## Steps

1. Verify source image exists
2. Execute: `docker tag <source> <target>`
3. Show both images with `docker images`

## Examples

### Version Tagging
User: `/docker:tag myapp:latest myapp:v1.0.0`
→ `docker tag myapp:latest myapp:v1.0.0`
→ Creates version tag

### Registry Tagging
User: `/docker:tag myapp ghcr.io/username/myapp:latest`
→ `docker tag myapp ghcr.io/username/myapp:latest`
→ Prepares for push to GitHub Container Registry

User: `/docker:tag myapp 123456789.dkr.ecr.us-east-1.amazonaws.com/myapp:v1`
→ Prepares for push to AWS ECR

### Release Workflow
User: `/docker:tag myapp:abc123 myapp:latest`
→ Tags specific build as latest

User: `/docker:tag myapp:latest myapp:stable`
→ Promotes latest to stable

## Tag Naming Conventions

| Convention | Example | Use Case |
|------------|---------|----------|
| Semantic version | `v1.2.3` | Releases |
| Git SHA | `abc123f` | CI builds |
| Environment | `dev`, `staging`, `prod` | Deployments |
| Date | `2024-01-15` | Nightly builds |
| Branch | `feature-auth` | Feature testing |

## Registry Formats

| Registry | Tag Format |
|----------|------------|
| Docker Hub | `username/image:tag` |
| GitHub | `ghcr.io/owner/image:tag` |
| AWS ECR | `<account>.dkr.ecr.<region>.amazonaws.com/image:tag` |
| GCR | `gcr.io/project/image:tag` |

## Tips

- Tagging doesn't duplicate the image (same layers)
- Use specific tags for production, not `latest`
- Tag before push to registry
- Multiple tags can point to same image
- Use `/docker:images` to see all tags
