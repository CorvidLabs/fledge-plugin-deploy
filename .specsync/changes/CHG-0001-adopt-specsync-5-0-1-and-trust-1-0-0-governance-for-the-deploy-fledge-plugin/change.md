---
id: CHG-0001-adopt-specsync-5-0-1-and-trust-1-0-0-governance-for-the-deploy-fledge-plugin
state: implementing
type: migration
base_commit: b441d6b73365e0557369fe725c30e2c38ce086c4
---

# Adopt SpecSync 5.0.1 and Trust 1.0.0 governance for the Deploy Fledge plugin

## Intent

Adopt SpecSync 5.0.1 and Trust 1.0.0 governance for the Deploy Fledge plugin

## Affected Canonical Specs

- None

## Acceptance Criteria

- SpecSync strict check passes at explicit advisory threshold 0; all four integrations report installed; Trust doctor and verification pass.
- ShellCheck and safe deploy, rollback, and hook smoke tests remain green.

## No-spec Rationale

The migration documents existing Deploy behavior and adds governance configuration without changing runtime semantics.
