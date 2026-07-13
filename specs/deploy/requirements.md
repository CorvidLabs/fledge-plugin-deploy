---
spec: deploy.spec.md
---

## User Stories

- As a plugin author, I want a reference deploy/rollback command implementation and lane hook.

## Acceptance Criteria

### REQ-deploy-001

Deploy SHALL validate production versions and report all four simulated deployment stages.

### REQ-deploy-002

Dry-run mode SHALL avoid delays and external side effects while displaying the planned deployment.

### REQ-deploy-003

Rollback SHALL require interactive confirmation unless `--yes` is supplied.

### REQ-deploy-004

The post-lane hook SHALL report lane context and return non-zero for failure status.

## Constraints

- Commands are reference simulations and do not deploy or restore real artifacts.

## Out of Scope

- Cloud-provider authentication, artifact upload, traffic switching, and real health checks.
