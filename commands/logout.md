---
name: logout
description: Log out from a Docker registry
allowed-tools:
  - Bash
argument-hint: "[registry]"
---

# Docker Logout Command

Remove stored credentials for a Docker registry.

## Behavior

1. **Identify registry**: Docker Hub or specified registry
2. **Remove credentials**: Delete stored authentication
3. **Report**: Confirm successful logout

## Steps

1. Identify target registry (default: Docker Hub)
2. Execute: `docker logout <registry>`
3. Confirm credentials removed

## Examples

### Docker Hub
User: `/docker:logout`
→ `docker logout`
→ Removes Docker Hub credentials

### GitHub Container Registry
User: `/docker:logout ghcr.io`
→ `docker logout ghcr.io`
→ Removes GitHub registry credentials

### AWS ECR
User: `/docker:logout 123456789.dkr.ecr.us-east-1.amazonaws.com`
→ `docker logout 123456789.dkr.ecr.us-east-1.amazonaws.com`
→ Removes ECR credentials

### Google Container Registry
User: `/docker:logout gcr.io`
→ `docker logout gcr.io`
→ Removes GCR credentials

## Common Registries

| Registry | Logout Command |
|----------|----------------|
| Docker Hub | `docker logout` |
| GitHub | `docker logout ghcr.io` |
| AWS ECR | `docker logout <account>.dkr.ecr.<region>.amazonaws.com` |
| Google GCR | `docker logout gcr.io` |
| Azure ACR | `docker logout <name>.azurecr.io` |

## When to Logout

| Scenario | Action |
|----------|--------|
| Switching accounts | Logout then login with new credentials |
| Security concern | Logout from all registries |
| Shared machine | Logout after use |
| CI/CD cleanup | Logout at end of pipeline |
| Credential rotation | Logout then login with new token |

## Verify Logout

Check credentials are removed:
```bash
cat ~/.docker/config.json
```

After logout, the registry entry should be removed from the `auths` section.

## Logout All Registries

To remove all stored credentials:
```bash
# View all logged-in registries
cat ~/.docker/config.json | jq '.auths | keys'

# Logout from each
docker logout
docker logout ghcr.io
docker logout gcr.io
# etc.
```

## Tips

- Logout removes credentials from local storage
- Required before logging in with different account
- Does not affect running containers
- Good practice on shared/public machines
- Use `/docker:login` to authenticate again
