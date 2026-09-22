---
id: API-DES-002
type: DesignSpecification
subtype: API
status: Baseline
version: 1
title: Stock Reservation API specification
relations:
  specifies:
    - API-INV-001
  satisfies:
    - REQ-INV-001
  constrainedBy:
    - NFR-PERF-001
    - NFR-REL-001
    - SREQ-001
  uses:
    - DB-INV-001
---

# API-DES-002 — Stock Reservation API Specification

## Operations

| Operation | Method/path | Purpose |
|---|---|---|
| Reserve | `POST /api/stock-reservations` | Atomically reserve required quantities |
| Release | `POST /api/stock-reservations/{id}/release` | Idempotently release reservation |
| Detail | `GET /api/stock-reservations/{id}` | Query reservation state |

## Reserve request

Contains Order ID, one or more product/location/quantity items and idempotency identity. Quantity must be positive. All-or-nothing is the baseline policy.

## Concurrency semantics

A successful reserve means the requested quantity is committed and `available stock >= 0` remains true. Competing requests may receive structured conflict/insufficient-stock result; caller must not assume client-side pre-check reserves stock.

## Idempotency

Retry of the same logical reserve request must return/recover the same business outcome rather than create another reservation. Reusing an idempotency key for materially different payload is rejected.

## Release semantics

Release transitions Active reservation once. Repeated release of already released/expired reservation is handled idempotently according to contract and never increments stock twice.

## Error model

```text
VALIDATION_ERROR
INSUFFICIENT_STOCK
RESERVATION_NOT_FOUND
RESERVATION_NOT_ACTIVE
IDEMPOTENCY_CONFLICT
CONCURRENCY_CONFLICT
FORBIDDEN
```

## Verification

`TEST-INV-001` verifies functional behavior; `REL-TEST-001` verifies duplicate/concurrent/failure behavior; `PERF-001` verifies load/latency target.
