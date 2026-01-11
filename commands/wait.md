---
name: wait
description: Wait for containers to stop and return exit codes
allowed-tools:
  - Bash
argument-hint: "<container> [container...]"
---

# Docker Wait Command

Block until containers stop, then print their exit codes.

## Behavior

1. **Identify containers**: One or more by name or ID
2. **Block**: Wait until all containers stop
3. **Return**: Print exit code for each container

## Steps

1. Verify container(s) exist
2. Execute: `docker wait <container>`
3. Block until container exits
4. Display exit code

## Examples

User: `/docker:wait api`
→ `docker wait api`
→ Waits for api to stop, prints exit code (e.g., `0`)

User: `/docker:wait api db worker`
→ `docker wait api db worker`
→ Waits for all three, prints each exit code

User: `/docker:wait test-runner`
→ Waits for test container to finish
→ Returns `0` on success, non-zero on failure

## Exit Codes

| Code | Meaning |
|------|---------|
| 0 | Success |
| 1 | General error |
| 126 | Permission denied |
| 127 | Command not found |
| 137 | Killed (SIGKILL) |
| 143 | Terminated (SIGTERM) |

## Use Cases

### CI/CD Pipelines
```bash
# Run tests and wait for result
docker run -d --name tests myapp-tests
exit_code=$(docker wait tests)
if [ $exit_code -ne 0 ]; then
  echo "Tests failed!"
  exit 1
fi
```

### Sequential Operations
```bash
# Wait for db to initialize before starting app
docker wait db-init
docker start app
```

### Batch Processing
```bash
# Wait for all workers to complete
docker wait worker1 worker2 worker3
echo "All workers finished"
```

### Health Checks
```bash
# Wait and check result
docker run -d --name healthcheck myapp --check
result=$(docker wait healthcheck)
echo "Health check exit code: $result"
```

## Scripting Example

```bash
# Run container in background
docker run -d --name job myimage

# Do other work...

# Wait for completion and get exit code
exit_code=$(docker wait job)
echo "Job finished with code: $exit_code"

# Clean up
docker rm job
```

## Tips

- Blocks current process until container exits
- Useful in scripts for synchronization
- Returns immediately if container already stopped
- Multiple containers return codes in order
- Combine with `docker logs` to see output after wait
- Use in CI/CD to get test/build results
