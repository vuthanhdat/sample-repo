# API List

> Derived/inventory view. Trong SaaS thật, list này nên generate từ Deliverable registry.

| ID | API | Module | Related Requirement | Design Spec | Implement Task | Verification | Status |
|---|---|---|---|---|---|---|---|
| `API-ORD-001` | Order Command API | Order | `REQ-ORD-001..003` | `API-DES-001` | `TASK-ORD-BE-001` | `TEST-ORD-001`, `PERF-001`, `SEC-TEST-001` | Specified |
| `API-INV-001` | Stock Reservation API | Inventory | `REQ-INV-001` | `API-DES-002` | `TASK-INV-BE-001` | `TEST-INV-001`, `PERF-001`, `REL-TEST-001` | Specified |

## Required columns for project template

ID, name, module/service, protocol, consumer, requirement, design spec, auth policy, idempotency, implementation task, verification và lifecycle status.