---
id: DB-ORD-001
type: Deliverable
subtype: DatabaseArtifact
status: Specified
version: 1
title: Order Management database schema
relations:
  specifiedBy:
    - DBD-001
    - DBD-002
  implementsRequirements:
    - DREQ-001
    - REQ-ORD-001
    - REQ-ORD-003
  constrainedBy:
    - NFR-PERF-001
    - NFR-REL-001
---

# DB-ORD-001 — Order Management Database

## Planned database artifacts

- `orders`
- `order_lines`
- `order_status_history`
- approval/audit persistence required by finalized implementation boundary
- supporting indexes/constraints/migrations

## Source specifications

`DBD-001` defines semantic/logical model; `DBD-002` defines PostgreSQL physical baseline. Migration files are **implementation artifacts**, not the deliverable identity itself.

## Acceptance boundary

Deliverable is implemented when required schema/migrations exist and pass migration/integration tests; verified only after integrity/performance/recovery checks required by linked NFRs succeed.
