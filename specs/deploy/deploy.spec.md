---
module: deploy
version: 1
status: active
files:
  - bin/fledge-deploy
  - bin/fledge-rollback
  - hooks/lane-post

db_tables: []
depends_on: []
---

# Deploy

## Purpose

Provide reference simulated deploy and rollback commands plus a post-lane status hook that demonstrates fledge command and lifecycle integration without modifying an actual deployment target.

## Public API

| Surface | Behavior |
|---------|----------|
| deploy | Validate target/version, print four simulated stages, and support a no-delay dry run. |
| rollback | Confirm unless explicitly approved, then print simulated previous-version restoration and health check. |
| lane post hook | Print lane name, status, and optional run ID; return non-zero for a failed lane. |

## Invariants

1. Production deployment requires an explicit version.
2. Dry-run deployment skips simulated delays while retaining all planned output.
3. Rollback prompts only when confirmation was not supplied and stdin is interactive.
4. The reference commands simulate deployment state and do not contact an external target.
5. A post-lane failure status returns non-zero so fledge surfaces the hook failure.

## Behavioral Examples

```
Given a staging target and dry-run mode
When deploy runs
Then it reports preflight, build, push, and health-check stages without external side effects
```

## Error Cases

| Error | When | Behavior |
|-------|------|----------|
| Missing production version | Production deploy has no version | Report the requirement and exit non-zero. |
| Unknown argument | A command receives an unsupported flag | Report the argument and exit non-zero. |
| Declined rollback | Interactive confirmation is not affirmative | Print `Aborted` and exit successfully without simulated rollback. |
| Failed lane | Hook receives failure status | Print the failed status and exit non-zero. |

## Dependencies

- Bash
- fledge command packaging and lane environment variables

## Change Log

| Version | Date | Changes |
|---------|------|---------|
| 1 | 2026-07-12 | Document existing reference deploy, rollback, and hook behavior for SpecSync 5 adoption. |
