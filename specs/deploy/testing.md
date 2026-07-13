---
spec: deploy.spec.md
---

## Test Plan

### Integration Tests

- `shellcheck bin/* hooks/*`
- `bin/fledge-deploy --dry-run`
- `bin/fledge-rollback --yes`
- Run `hooks/lane-post` with a successful synthetic lane context.
