---
id: TASK-DATA-001
type: Task
status: Ready
version: 1
title: Implement Order and Inventory database foundation
relations:
  implements:
    - DB-ORD-001
    - DB-INV-001
  reads:
    - DREQ-001
    - DBD-001
    - DBD-002
    - NFR-PERF-001
    - NFR-REL-001
---

# TASK-DATA-001 — Implement Database Foundation

## Objective

Tạo schema/migration baseline cho `DB-ORD-001` và `DB-INV-001` theo logical/physical data design.

## Mandatory read set

- `DREQ-001`.
- `DBD-001`, `DBD-002`.
- `NFR-PERF-001`, `NFR-REL-001`.
- Functional requirements tạo ra core invariants: `REQ-ORD-001`, `REQ-ORD-003`, `REQ-INV-001`.

## Write set

- Database migrations/mappings realizing `DB-ORD-001`, `DB-INV-001`.
- Database-focused integration/concurrency tests.

## Done when

- Clean database migration succeeds.
- Upgrade migration from previous baseline succeeds where applicable.
- Constraints/indexes match `DBD-002`.
- Concurrent reservation test proves no negative available stock.
- Migration output is linked as implementation artifacts.
