---
id: DB-INV-001
type: Deliverable
subtype: DatabaseArtifact
status: Specified
version: 1
title: Inventory database schema
relations:
  specifiedBy:
    - DBD-001
    - DBD-002
  implementsRequirements:
    - DREQ-001
    - REQ-INV-001
  constrainedBy:
    - NFR-PERF-001
    - NFR-REL-001
---

# DB-INV-001 — Inventory Database

## Planned database artifacts

- `stock_balances`
- `stock_reservations`
- `inventory_transactions`
- indexes/constraints phục vụ expiration, order lookup và concurrency-safe update

## Core invariant

Database/application design phải bảo vệ `available stock >= 0` dưới concurrent reservation. Không coi passing single-thread unit test là đủ bằng chứng.

## Implementation artifact examples

- EF Core/PostgreSQL migrations.
- Entity mappings.
- Index DDL.
- Data backfill script nếu migration cần.

Các file trên realize `DB-INV-001`; chúng không thay thế identity/version của deliverable.
