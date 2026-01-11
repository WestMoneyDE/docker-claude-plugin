---
name: push
description: Push Docker images to registries
allowed-tools:
  - Bash
argument-hint: "<image:tag> [registry-url]"
---

# Docker Push Command

Push Docker images to container registries (Docker Hub, ECR, GCR, etc.).

## Supported Registries

| Registry | Format |
|----------|--------|
| Docker Hub | `username/image:tag` |
| AWS ECR | `123456789.dkr.ecr.region.amazonaws.com/image:tag` |
| GCR | `gcr.io/project/image:tag` |
| GitHub | `ghcr.io/owner/image:tag` |
| Custom | `registry.example.com/image:tag` |

## Behavior

1. **Validate image**: Check if image exists locally
2. **Check authentication**: Verify logged into registry
3. **Tag if needed**: Apply registry prefix
4. **Push**: Upload image to registry
5. **Report**: Show push result and image URL

## Steps

1. Check if image exists: `docker images <image>`
2. If registry specified, tag image appropriately:
   ```bash
   docker tag <image> <registry>/<image>:<tag>
   ```
3. Push to registry:
   ```bash
   docker push <registry>/<image>:<tag>
   ```
4. Show the full image URL for pulling

## Examples

User: `/docker:push myapp:v1`
→ Assumes Docker Hub: `docker push myapp:v1`

User: `/docker:push myapp:v1 ghcr.io/username`
→ Tag and push to GitHub Container Registry

User: `/docker:push myapp:latest 123456789.dkr.ecr.us-east-1.amazonaws.com`
→ Tag and push to AWS ECR

## Authentication

If not logged in, show how to authenticate:

**Docker Hub:**
```bash
docker login
```

**AWS ECR:**
```bash
aws ecr get-login-password | docker login --username AWS --password-stdin <account>.dkr.ecr.<region>.amazonaws.com
```

**GitHub:**
```bash
echo $GITHUB_TOKEN | docker login ghcr.io -u USERNAME --password-stdin
```

## Tips

- Check authentication before pushing
- Remind about tagging with registry prefix
- Show the pull command after successful push
- Warn about pushing `latest` tag to production
