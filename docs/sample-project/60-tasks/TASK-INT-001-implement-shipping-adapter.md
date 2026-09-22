---
id: TASK-INT-001
type: Task
status: Ready
version: 1
title: Implement shipping provider adapter
relations:
  implements:
    - INT-SHP-001
  reads:
    - IREQ-001
    - INT-DES-001
    - SREQ-001
    - NFR-REL-001
    - NFR-OPS-001
  verifiedBy:
    - TEST-SHP-001
---

# TASK-INT-001 — Implement Shipping Provider Adapter

## Objective

Implement `INT-SHP-001` without leaking provider-specific DTO/errors into core Order application/domain.

## Mandatory read set

- `IREQ-001`.
- `INT-DES-001`.
- `SREQ-001`, `NFR-REL-001`, `NFR-OPS-001`.

## Write set

- Shipping port implementation/provider adapter.
- Provider configuration/secret reference integration.
- Mapping, timeout/retry/idempotency handling.
- Contract/integration tests.

## Done when

- Create shipment is duplicate-safe.
- Provider errors map to canonical categories.
- Sensitive recipient data is not exposed in logs.
- Timeout/retry behavior is observable.
- `TEST-SHP-001` passes against provider sandbox/stub contract.
