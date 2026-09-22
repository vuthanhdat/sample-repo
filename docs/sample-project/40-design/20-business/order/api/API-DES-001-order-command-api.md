---
id: API-DES-001
type: DesignSpecification
subtype: API
status: Baseline
version: 1
title: Order Command API specification
relations:
  specifies:
    - API-ORD-001
  satisfies:
    - REQ-ORD-001
    - REQ-ORD-002
    - REQ-ORD-003
  constrainedBy:
    - NFR-PERF-001
    - NFR-REL-001
    - SREQ-001
---

# API-DES-001 — Order Command API Specification

## API boundary

Base path: `/api/orders`.

| Operation | Method/path | Main purpose |
|---|---|---|
| Create | `POST /api/orders` | Create Order |
| Detail | `GET /api/orders/{id}` | Current state + summary |
| Approve | `POST /api/orders/{id}/approve` | Apply approval decision |
| Cancel | `POST /api/orders/{id}/cancel` | Request valid cancellation |
| History | `GET /api/orders/{id}/status-history` | Paged business timeline |

## Command contract rules

- Command endpoint accepts `Idempotency-Key` when retry can create duplicate effect.
- Optimistic concurrency uses explicit expected version for state-sensitive action where required.
- Business validation error returns stable error code, not localized UI string as machine contract.
- Authorization happens before privileged business effect but does not replace domain validation.

## Create Order

Required fields: Customer ID, one or more Order Lines, shipping data required by current flow. Server computes authoritative monetary totals according to pricing scope; client-provided total is never trusted as final business value.

## Approve Order

Request contains decision comment/expected version as applicable. Response returns Order ID, resulting status and version. Creator/approver/threshold constraints reference canonical business rules rather than duplicated constants.

## Error model

```text
VALIDATION_ERROR
ORDER_NOT_FOUND
INVALID_ORDER_STATE
APPROVAL_REQUIRED
FORBIDDEN
CONCURRENCY_CONFLICT
DEPENDENCY_UNAVAILABLE
```

Each error has stable `code`, human message, correlation ID and optional field errors.

## Pagination

Status history uses cursor or stable `(changedAt,id)` pagination; no unbounded history response.

## Observability

Every command has correlation ID, latency metric, result category and structured log without sensitive shipping payload.
