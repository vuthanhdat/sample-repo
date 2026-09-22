---
id: API-ORD-001
type: Deliverable
subtype: API
status: Specified
version: 2
title: Order Command API
relations:
  introducedBy:
    - DES-ORD-001
  specifiedBy:
    - API-DES-001
  implementsRequirements:
    - REQ-ORD-001
    - REQ-ORD-002
    - REQ-ORD-003
  constrainedBy:
    - NFR-PERF-001
    - NFR-REL-001
    - SREQ-001
  implementedBy:
    - TASK-ORD-BE-001
  verifiedBy:
    - TEST-ORD-001
    - PERF-001
    - REL-TEST-001
    - SEC-TEST-001
---

# API-ORD-001 — Order Command API

## Purpose

Cung cấp command/query boundary cho tạo, approve, cancel và truy vấn Order. Deliverable ID này đại diện cho API product; canonical detailed contract nằm ở `API-DES-001`.

## Operations

- `POST /api/orders`
- `POST /api/orders/{orderId}/approve`
- `POST /api/orders/{orderId}/cancel`
- `GET /api/orders/{orderId}`
- `GET /api/orders/{orderId}/status-history`

## Contract responsibilities

- Validate command input và state transition.
- Apply approval/authorization policy.
- Preserve idempotency/concurrency semantics.
- Persist Order/history through owned data boundary.
- Publish `EVT-ORD-001` according to event design.
- Meet linked performance/reliability/security requirements.

## Design sources

`DES-ORD-001` explains why this deliverable exists; `API-DES-001` defines detailed API behavior; `SEC-DES-001`, `DBD-*`, `EVT-DES-001` constrain related implementation aspects.

## Traceability

```text
REQ/NFR/SREQ
   ↓
DES-ORD-001
   ↓ introduces
API-ORD-001
   ↑ specified-by API-DES-001
   ↓ implemented-by TASK-ORD-BE-001
   ↓ verified-by TEST-ORD-001 / PERF-001 / REL-TEST-001 / SEC-TEST-001
```
