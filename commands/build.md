---
name: build
description: Build Docker images from Dockerfile
allowed-tools:
  - Bash
  - Read
  - Glob
argument-hint: "[image-name:tag] [--no-cache] [path-to-dockerfile]"
---

# Docker Build Command

Build Docker images from a Dockerfile in the current directory or specified path.

## Behavior

1. **Find Dockerfile**: Look for Dockerfile in current directory, or use specified path
2. **Determine image name**: Use provided name or derive from directory name
3. **Execute build**: Run `docker build` with appropriate options
4. **Show results**: Display build output and final image info

## Steps

1. Check if Dockerfile exists in current directory or specified path
2. If no image name provided, suggest one based on directory name
3. Run the build command:
   ```bash
   docker build -t <image-name>:<tag> <context-path>
   ```
4. If `--no-cache` specified, add `--no-cache` flag
5. Show the resulting image with `docker images <image-name>`

## Examples

User: `/docker:build`
→ Auto-detect Dockerfile, build with directory name as image name

User: `/docker:build myapp:v1`
→ Build and tag as myapp:v1

User: `/docker:build myapp:latest --no-cache`
→ Build without using cache

User: `/docker:build myapp ./services/api`
→ Build from specific directory

## Tips

- Always show build progress to the user
- If build fails, analyze the error and suggest fixes
- After successful build, show image size
- Suggest using `.dockerignore` if build context is large
