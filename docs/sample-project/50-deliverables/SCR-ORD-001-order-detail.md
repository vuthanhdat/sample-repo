---
id: SCR-ORD-001
type: Deliverable
subtype: Screen
status: Specified
version: 1
title: Order Detail Screen
relations:
  introducedBy:
    - DES-ORD-001
  implementsRequirements:
    - REQ-ORD-003
  implementedBy:
    - TASK-ORD-FE-001
---

# SCR-ORD-001 — Order Detail Screen

## Purpose

Hiển thị thông tin Order, trạng thái hiện tại, approval state và timeline thay đổi trạng thái.

## Main sections

- Order summary.
- Customer and shipping information.
- Order lines.
- Current status.
- Approval information.
- Status timeline.

## Main actions

- Approve Order khi user có quyền và Order đang `PendingApproval`.
- Cancel Order khi state cho phép.
- Refresh latest status/history.

## Data dependencies

- `API-ORD-001`.

## Traceability

`REQ-ORD-003` → `DES-ORD-001` → `SCR-ORD-001` → `TASK-ORD-FE-001`.
