---
change: CHG-0001-adopt-specsync-5-0-1-and-trust-1-0-0-governance-for-the-deploy-fledge-plugin
artifact: testing
---

# Testing

- `shellcheck bin/* hooks/*`
- `bin/fledge-deploy --dry-run`
- `bin/fledge-rollback --yes`
- Synthetic successful `hooks/lane-post`
- `specsync check --strict --force` at advisory threshold 0
- `fledge trust doctor` and `fledge trust verify`
