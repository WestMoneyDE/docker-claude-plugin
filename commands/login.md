---
name: login
description: Log in to a Docker registry
allowed-tools:
  - Bash
argument-hint: "[registry] [--username] [--password-stdin]"
---

# Docker Login Command

Authenticate with a Docker registry to push and pull private images.

## Behavior

1. **Identify registry**: Docker Hub or specified registry
2. **Authenticate**: Use credentials to log in
3. **Store credentials**: Save for future operations
4. **Report**: Confirm successful login

## Options

| Option | Description |
|--------|-------------|
| `--username, -u` | Username for registry |
| `--password, -p` | Password (not recommended, use stdin) |
| `--password-stdin` | Read password from stdin (secure) |

## Steps

1. Identify target registry (default: Docker Hub)
2. Prompt for credentials or use provided
3. Execute: `docker login <registry>`
4. Confirm authentication success

## Examples

### Docker Hub
User: `/docker:login`
→ `docker login`
→ Prompts for Docker Hub credentials

User: `/docker:login --username myuser`
→ `docker login -u myuser`
→ Prompts for password only

### GitHub Container Registry
User: `/docker:login ghcr.io`
→ `docker login ghcr.io`
→ Prompts for GitHub credentials

```bash
echo $GITHUB_TOKEN | docker login ghcr.io -u USERNAME --password-stdin
```

### AWS ECR
User: `/docker:login 123456789.dkr.ecr.us-east-1.amazonaws.com`
→ Shows ECR login command:
```bash
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 123456789.dkr.ecr.us-east-1.amazonaws.com
```

### Google Container Registry
User: `/docker:login gcr.io`
→ Shows GCR login command:
```bash
gcloud auth configure-docker
# or
cat key.json | docker login -u _json_key --password-stdin gcr.io
```

## Registry Authentication

| Registry | Method |
|----------|--------|
| Docker Hub | Username + password/token |
| GitHub (ghcr.io) | Username + personal access token |
| AWS ECR | AWS CLI + `get-login-password` |
| Google GCR | gcloud or service account JSON |
| Azure ACR | `az acr login` or service principal |

## Credentials Storage

Credentials are stored in:
- Linux: `~/.docker/config.json`
- macOS: Keychain (via docker-credential-osxkeychain)
- Windows: Credential Manager

## Security Tips

- Never use `-p` flag (exposes password in shell history)
- Always use `--password-stdin` for scripts
- Use access tokens instead of passwords
- Rotate credentials regularly
- Use credential helpers for secure storage

## Tips

- Login persists until logout or credential expiry
- Required before pushing to private registries
- Check login status: `cat ~/.docker/config.json`
- Use `/docker:logout` to remove credentials
