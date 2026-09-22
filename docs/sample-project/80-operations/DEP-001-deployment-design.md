---
id: DEP-001
type: OperationsDesign
subtype: Deployment
status: Baseline
version: 1
title: Deployment topology and release design
relations:
  constrainedBy:
    - NFR-REL-001
    - NFR-OPS-001
    - SREQ-001
  deploys:
    - API-ORD-001
    - API-INV-001
    - JOB-INV-001
    - SCR-ORD-001
---

# DEP-001 — Deployment Design

## Runtime topology

Baseline sample assumes:

```text
Frontend static/web app
        ↓ HTTPS
Backend API deployment
   ├── Order module
   ├── Inventory module
   └── Integration adapters
        ↓
PostgreSQL

Background worker deployment
   └── JOB-INV-001 and async/outbox processing
```

MVP may deploy modules together as a modular monolith; logical ownership remains separate even when process/container is shared.

## Environments

- Local/dev.
- Integration/test.
- Staging/pre-production.
- Production.

Environment-specific endpoint/credential/configuration is externalized; source code artifact should be promoted without rebuilding business logic differently per environment where feasible.

## Release strategy

- Database migration compatibility checked before application rollout.
- Additive/backward-compatible schema preferred for rolling deployment.
- Health/readiness gate before traffic cutover.
- Critical smoke tests after deploy.
- Rollback or roll-forward decision documented for schema-affecting release.

## Secrets

Secrets are injected from secret/config provider and never exported into project documents or repository plaintext.

## Recovery

Production backup/restore process must support `NFR-REL-001` RPO/RTO. Restore verification is part of operational readiness, not assumed from backup job existence.

## Evidence

Each release records application revision, migration revision, configuration baseline, deployment result and verification run links.
