---
id: DES-INV-001
type: DesignDecision
status: Baseline
version: 2
title: Stock reservation solution
relations:
  satisfies:
    - REQ-INV-001
  constrainedBy:
    - NFR-PERF-001
    - NFR-REL-001
    - DREQ-001
    - SREQ-001
  introduces:
    - API-INV-001
    - DB-INV-001
    - JOB-INV-001
---

# DES-INV-001 — Stock Reservation Solution

## Context

`REQ-INV-001` yêu cầu reserve/release stock nhất quán, không cho available stock âm và phải recover được khi Order flow bị bỏ dở. Reliability và concurrency là architecture driver, không chỉ là implementation detail.

## Decision

Inventory module sở hữu StockBalance/StockReservation/InventoryTransaction và cung cấp canonical application operations qua Stock Reservation API. Reservation là idempotent theo logical request identity. Expired active reservation được xử lý bởi background job nhưng job phải gọi cùng canonical application/domain operation, không bypass invariant bằng direct SQL business update.

## Deliverables introduced

- `API-INV-001` — Stock Reservation API.
- `DB-INV-001` — Inventory Database.
- `JOB-INV-001` — Release Expired Reservations Job.

## Detailed specifications

- `API-DES-002` specifies API contract/concurrency/idempotency.
- `DBD-001`, `DBD-002` specify logical/physical persistence.
- `JOB-DES-001` specifies schedule/selection/retry/rerun/concurrency/observability.
- `CFG-DES-001` defines reservation TTL configuration.

## Key design rules

1. Reservation kiểm tra + commit quantity trong transaction/concurrency boundary bảo vệ `BR-INV-001`.
2. No partial reservation dưới current all-or-nothing policy.
3. Retry same logical reserve/release không gây double update.
4. Expiry job race với fulfillment/cancel vẫn chỉ có một valid terminal transition.
5. Inventory history đủ để reconcile quantity và không được thay bằng generic technical log.

## Traceability

```text
REQ-INV-001 + NFR/DREQ
        ↓
DES-INV-001
   ├─ API-INV-001 ← API-DES-002
   ├─ DB-INV-001  ← DBD-001/DBD-002
   └─ JOB-INV-001 ← JOB-DES-001
```
