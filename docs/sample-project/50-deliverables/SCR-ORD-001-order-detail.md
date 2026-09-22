---
id: SCR-ORD-001
type: Deliverable
subtype: Screen
status: Specified
version: 2
title: Order Detail Screen
relations:
  introducedBy:
    - DES-ORD-001
  specifiedBy:
    - SCR-DES-001
  implementsRequirements:
    - REQ-ORD-003
  constrainedBy:
    - NFR-PERF-001
    - SREQ-001
  uses:
    - API-ORD-001
  implementedBy:
    - TASK-ORD-FE-001
---

# SCR-ORD-001 — Order Detail Screen

## Purpose

Hiển thị Order summary, current state, approval/inventory information và status timeline; expose action theo state/permission. Detailed interaction/layout/error/accessibility rules nằm ở `SCR-DES-001`.

## Main sections

- Order summary.
- Customer/shipping summary theo permission/data policy.
- Order lines.
- Current status.
- Approval information.
- Inventory reservation summary.
- Status timeline.

## Main actions

- Approve Order khi permission/state/business rule cho phép.
- Cancel Order khi policy cho phép.
- Refresh/recover from concurrency conflict.

UI action visibility không thay backend authorization.

## Traceability

`REQ-ORD-003` → `DES-ORD-001` → `SCR-ORD-001` ← `SCR-DES-001` → `TASK-ORD-FE-001`.
