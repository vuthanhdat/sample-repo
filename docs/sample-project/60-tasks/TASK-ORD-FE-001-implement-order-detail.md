---
id: TASK-ORD-FE-001
type: Task
status: Ready
version: 1
title: Implement Order Detail Screen
relations:
  implements:
    - SCR-ORD-001
  reads:
    - REQ-ORD-003
    - AC-ORD-003-01
    - AC-ORD-003-02
    - DES-ORD-001
    - API-ORD-001
---

# TASK-ORD-FE-001 — Implement Order Detail Screen

## Objective

Implement `SCR-ORD-001` để user xem Order, current status và status timeline.

## Mandatory read set

- `REQ-ORD-003` — Track Order Status.
- `AC-ORD-003-01`, `AC-ORD-003-02`.
- `DES-ORD-001`.
- `API-ORD-001` contract.

## Write set

- `SCR-ORD-001` frontend implementation.
- UI tests trực tiếp cho screen.

## Expected output

- Order summary.
- Current status.
- Approval information.
- Status timeline.
- Action controls theo state/permission.

## Done when

- Screen render đúng current status và timeline.
- UI action chỉ xuất hiện khi state cho phép.
- Contract với `API-ORD-001` được tuân thủ.
