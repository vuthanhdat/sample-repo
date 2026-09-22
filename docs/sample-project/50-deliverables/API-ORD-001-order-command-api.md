---
id: API-ORD-001
type: Deliverable
subtype: API
status: Specified
version: 1
title: Order Command API
relations:
  introducedBy:
    - DES-ORD-001
  implementsRequirements:
    - REQ-ORD-001
    - REQ-ORD-002
    - REQ-ORD-003
  implementedBy:
    - TASK-ORD-BE-001
  verifiedBy:
    - TEST-ORD-001
---

# API-ORD-001 — Order Command API

## Purpose

Cung cấp command boundary cho các thao tác tạo, approve và cancel Order.

## Planned operations

- `POST /orders`
- `POST /orders/{orderId}/approve`
- `POST /orders/{orderId}/cancel`
- `GET /orders/{orderId}`
- `GET /orders/{orderId}/status-history`

## Contract responsibilities

- Validate command input.
- Enforce valid state transition.
- Apply approval policy.
- Trigger stock reservation/release coordination.
- Record status history.
- Publish `EVT-ORD-001` khi status thay đổi theo policy.

## Traceability

`DES-ORD-001` → `API-ORD-001` → `TASK-ORD-BE-001` → `TEST-ORD-001`.
