---
id: EVT-ORD-001
type: Deliverable
subtype: Event
status: Specified
version: 2
title: Order Status Changed Event
relations:
  introducedBy:
    - DES-ORD-001
  specifiedBy:
    - EVT-DES-001
  implementsRequirements:
    - REQ-ORD-003
  constrainedBy:
    - NFR-REL-001
  implementedBy:
    - TASK-ORD-BE-001
  verifiedBy:
    - TEST-ORD-001
    - REL-TEST-001
---

# EVT-ORD-001 — Order Status Changed Event

## Purpose

Thông báo một committed Order state transition cho consumer bên ngoài Order Management. Canonical schema/delivery/versioning assumptions nằm ở `EVT-DES-001`.

## Minimum semantic payload

- event ID/schema version;
- Order ID;
- previous/new status;
- occurred time;
- actor/source;
- correlation/causation metadata.

## Delivery semantics

Consumer phải chịu được duplicate/delayed delivery theo `EVT-DES-001`; global ordering không được giả định nếu contract không quy định.

## Traceability

`REQ-ORD-003` → `DES-ORD-001` → `EVT-ORD-001` ← `EVT-DES-001` → implementation + reliability verification.
