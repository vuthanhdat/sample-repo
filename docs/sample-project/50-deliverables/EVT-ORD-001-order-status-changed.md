---
id: EVT-ORD-001
type: Deliverable
subtype: Event
status: Specified
version: 1
title: Order Status Changed Event
relations:
  introducedBy:
    - DES-ORD-001
  implementsRequirements:
    - REQ-ORD-003
  implementedBy:
    - TASK-ORD-BE-001
---

# EVT-ORD-001 — Order Status Changed Event

## Purpose

Thông báo cho các consumer bên ngoài Order Management khi trạng thái Order thay đổi.

## Minimum payload

- `eventId`
- `orderId`
- `previousStatus`
- `newStatus`
- `occurredAt`
- `causationId`
- `actor/source`

## Publishing rule

Event được publish sau một status transition hợp lệ. Consumer không được coi thứ tự delivery là tuyệt đối nếu contract chưa đảm bảo ordering; event phải có identity để hỗ trợ idempotent consumption.

## Traceability

`REQ-ORD-003` → `DES-ORD-001` → `EVT-ORD-001` → `TASK-ORD-BE-001`.
