---
id: ARCH-002
type: ArchitectureSpecification
status: Baseline
version: 1
title: Component and dependency boundaries
relations:
  refines:
    - ARCH-001
  constrainedBy:
    - NFR-REL-001
    - SREQ-001
---

# ARCH-002 — Component & Dependency Boundaries

## Runtime modules

```text
Order.Api
  ↓
Order.Application
  ↓
Order.Domain

Order.Infrastructure → implements ports defined inward

Inventory.Api
  ↓
Inventory.Application
  ↓
Inventory.Domain

Inventory.Infrastructure → implements ports defined inward
```

Shipping integration is an adapter owned outside core Order domain logic.

## Dependency rules

1. Domain không phụ thuộc API, Infrastructure hoặc provider SDK.
2. Application orchestration phụ thuộc domain + abstraction/port, không phụ thuộc concrete infrastructure adapter.
3. API/controller không chứa business state-transition logic.
4. Job runner không update business tables trực tiếp; nó gọi application use case.
5. Order module không reference Inventory database mapping/table classes.
6. External provider adapter không leak provider DTO vào core application/domain contract.

## Cross-cutting concerns

Authentication, logging, metrics, tracing, transaction plumbing và validation framework có thể dùng shared infrastructure/middleware nhưng business decisions phải nằm ở owner module.

## Enforcement candidates

- architecture tests cho reference direction/module boundary;
- static analysis cho dependency cycles;
- contract tests cho cross-module/external contracts;
- code owners/review policy cho owner boundaries.

## Change rule

Nếu một task cần phá boundary này, đó là architecture change và phải update `ARCH-002`/ADR trước hoặc cùng change request; không được coi là refactor implementation nhỏ.
