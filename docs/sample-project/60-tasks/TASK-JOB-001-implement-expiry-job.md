---
id: TASK-JOB-001
type: Task
status: Ready
version: 1
title: Implement expired reservation release job
relations:
  implements:
    - JOB-INV-001
  reads:
    - REQ-INV-001
    - NFR-REL-001
    - NFR-PERF-001
    - JOB-DES-001
    - DBD-002
  dependsOn:
    - TASK-DATA-001
  verifiedBy:
    - REL-TEST-001
---

# TASK-JOB-001 — Implement Expired Reservation Release Job

## Objective

Implement `JOB-INV-001` exactly through the canonical Inventory application operation defined by `JOB-DES-001`.

## Mandatory read set

- `JOB-DES-001`.
- `REQ-INV-001`, `NFR-REL-001`, `NFR-PERF-001`.
- `DBD-002`, `DB-INV-001`.

## Write set

- Job scheduler/worker implementation for `JOB-INV-001`.
- Job-specific configuration/metrics/tests.
- No direct ad-hoc business table update that bypasses Inventory invariant.

## Done when

- Schedule/selection/paging/retry/rerun policy is implemented.
- Duplicate/concurrent execution is safe.
- Metrics defined in `JOB-DES-001` exist.
- `REL-TEST-001` relevant scenarios pass.
