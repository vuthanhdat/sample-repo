---
id: NFR-REL-001
type: NonFunctionalRequirement
category: AvailabilityReliability
status: Baseline
version: 1
title: Availability, consistency and recovery targets
relations:
  constrains:
    - API-ORD-001
    - API-INV-001
    - DB-ORD-001
    - DB-INV-001
    - JOB-INV-001
---

# NFR-REL-001 — Availability & Reliability

## Availability target

- Monthly service availability target: **99.9%** cho Order/Inventory APIs, loại trừ approved maintenance windows.
- Không có yêu cầu multi-region active-active ở baseline sample.

## Data correctness invariants

1. Một successful Order command không được bị mất sau khi trả success.
2. Available stock không được âm do concurrent reservations.
3. Retry command/event không được tạo duplicate business effect.
4. Order current state và status history phải reconcile được.
5. Release reservation phải idempotent.

## Recovery targets

| Measure | Target |
|---|---|
| RPO | ≤ 5 minutes |
| RTO | ≤ 60 minutes |
| Failed background item | Retry theo policy, sau đó dead-letter/manual action |
| External dependency unavailable | Fail/degrade có kiểm soát; không corrupt internal state |

## Failure handling requirements

- External call timeout/retry phải bounded và có backoff.
- Message consumption phải idempotent.
- Long-running/background operation phải có retry count, next-attempt time, last error và terminal state.
- Partial failure qua nhiều subsystem phải có compensation/reconciliation strategy thay vì distributed transaction giả định.

## Verification

`REL-TEST-001` cover retry/idempotency, concurrent reservation, process restart và selected dependency failure scenarios.
