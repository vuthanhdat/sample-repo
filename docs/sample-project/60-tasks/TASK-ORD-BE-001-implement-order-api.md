---
id: TASK-ORD-BE-001
type: Task
status: Ready
version: 2
title: Implement Order Command API and event
relations:
  implements:
    - API-ORD-001
    - EVT-ORD-001
  reads:
    - REQ-ORD-001
    - REQ-ORD-002
    - REQ-ORD-003
    - BR-ORD-001
    - AC-ORD-001-01
    - AC-ORD-001-02
    - AC-ORD-002-01
    - AC-ORD-002-02
    - AC-ORD-003-01
    - AC-ORD-003-02
    - NFR-PERF-001
    - NFR-REL-001
    - NFR-OPS-001
    - SREQ-001
    - DREQ-001
    - DES-ORD-001
    - ARCH-001
    - ARCH-002
    - API-DES-001
    - EVT-DES-001
    - SEC-DES-001
    - CFG-DES-001
    - DB-ORD-001
  dependsOn:
    - TASK-DATA-001
  verifiedBy:
    - TEST-ORD-001
    - PERF-001
    - REL-TEST-001
    - SEC-TEST-001
---

# TASK-ORD-BE-001 — Implement Order Command API and Event

## Objective

Implement `API-ORD-001` và `EVT-ORD-001` theo full requirement/design baseline, không chỉ functional happy path.

## Mandatory context

Task runner phải resolve exact baseline versions của Functional Requirements/Rules/AC, applicable NFR/Data/Security requirements, architecture boundaries, API/Event/Security/Configuration design và `DB-ORD-001` contract.

## Write set

- Implementation realizing `API-ORD-001`.
- Implementation realizing `EVT-ORD-001`.
- Direct unit/integration/contract tests for these deliverables.

Database schema change ngoài `DB-ORD-001` baseline hoặc deliverable mới cần approved change request.

## Expected outputs

- Order command/query application handlers/endpoints.
- Approval/state transition logic.
- Status history persistence use.
- Idempotency/concurrency handling.
- Transactional/recoverable event publishing according to `EVT-DES-001`.
- Structured observability/audit hooks.

## Done when

- Functional verification passes.
- Applicable reliability/security checks pass.
- Performance evidence required for release is green or explicitly tracked before release gate.
- Architecture boundary tests pass.
- Task result links source/commit/PR/test evidence to `API-ORD-001` and `EVT-ORD-001`.
