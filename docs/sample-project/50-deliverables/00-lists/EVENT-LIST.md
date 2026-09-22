# Event List

> Derived/inventory view cho domain/integration events mà hệ thống publish hoặc consume.

| ID | Event | Direction | Producer | Consumers | Requirement | Design Spec | Implement Task | Status |
|---|---|---|---|---|---|---|---|---|
| `EVT-ORD-001` | Order Status Changed | Outbound | Order Module | External/Internal subscribers | `REQ-ORD-003` | `EVT-DES-001` | `TASK-ORD-BE-001` | Specified |

## Required columns

ID, name, direction, producer, consumer(s), topic/channel, schema version, delivery guarantee, ordering/idempotency rule, requirement, design spec, task, verification và status.