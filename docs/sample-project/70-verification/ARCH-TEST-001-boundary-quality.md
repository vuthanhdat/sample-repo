---
id: ARCH-TEST-001
type: VerificationDefinition
subtype: ArchitectureMaintainability
status: Baseline
version: 1
title: Architecture boundary and maintainability verification
relations:
  verifies:
    - NFR-MNT-001
    - ARCH-002
---

# ARCH-TEST-001 — Architecture Boundary & Maintainability Verification

## Automated rules

1. Domain projects/modules do not reference API/Infrastructure/provider SDK layers.
2. Order module does not reference Inventory infrastructure/database implementation.
3. No forbidden circular project/module dependency.
4. API/controller/job scheduler types do not become the only owner of domain business decisions.
5. Architecture/static quality gate checks agreed complexity/duplication/dependency violations.

## Contract/regression checks

Public API/event contracts must not change incompatibly without explicit version/change record. Refactor tasks run required regression suites and preserve behavior unless linked requirement/change states otherwise.

## Evidence

Architecture test/static-analysis result is captured per target revision. A waiver must reference an approved architecture/change decision with owner and expiry/review condition; a passing E2E test cannot override architecture violation by itself.
