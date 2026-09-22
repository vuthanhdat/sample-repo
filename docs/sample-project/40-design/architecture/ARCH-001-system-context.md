---
id: ARCH-001
type: ArchitectureSpecification
status: Baseline
version: 1
title: System context and ownership boundaries
relations:
  supports:
    - CAP-ORD-001
    - CAP-INV-001
  constrainedBy:
    - NFR-PERF-001
    - NFR-REL-001
    - SREQ-001
---

# ARCH-001 — System Context & Ownership Boundaries

## Architecture drivers

- Order state và inventory quantity phải có owner rõ ràng.
- Retry/idempotency quan trọng hơn coupling thấp thuần túy.
- External shipping failure không được corrupt internal Order state.
- Online commands cần đáp ứng NFR latency trong khi history/audit vẫn đầy đủ.

## System context

```text
Customer / Operations UI
          |
          v
   Order Management
      |        |
      |        +----> Event/Notification consumers
      |
      +----> Inventory
      |
      +----> Shipping Adapter ----> External Shipping Provider

PostgreSQL stores are owned by application modules; external systems do not access internal tables.
```

## Ownership

| Boundary | Owns | Exposes |
|---|---|---|
| Order Management | Order, OrderLine, status/approval history | Order API, order events |
| Inventory | Stock balance, reservation, inventory transaction | Reservation API, inventory events |
| Shipping Adapter | Provider mapping/request state only | Canonical shipping interface |

## Architectural rules

1. Module khác không trực tiếp update table thuộc owner khác.
2. Cross-module communication phải qua application contract/API/event đã baseline.
3. Business rules nằm ở domain/application boundary, không nằm riêng trong controller/UI/job scheduler.
4. External provider DTO được anti-corruption adapter map sang canonical model.
5. Background job chỉ orchestration/scheduling; business state transition vẫn gọi application/domain service.

## Related design products

- `DBD-001`, `DBD-002` — data/database design.
- `API-DES-001` — Order API design.
- `SCR-DES-001` — Order Detail screen design.
- `JOB-DES-001` — reservation expiry job.
- `EVT-DES-001` — order event contract.
- `INT-DES-001` — shipping integration.
- `SEC-DES-001` — security design.
