---
id: BE-ARCH-001
type: CommonDesign
scope: Common
status: Baseline
version: 1
title: Backend Architecture and Design Rules
appliesTo:
  - Backend
---

# BE-ARCH-001 — Backend Architecture and Design Rules

## Responsibility split

- Controller/Endpoint: protocol mapping, authentication context, request/response conversion; không chứa business logic.
- Application Use Case: orchestration một use case, transaction boundary, authorization policy invocation, gọi domain và ports.
- Domain: invariant, entity/value object, domain service, state transition và business rule.
- Infrastructure: database, message broker, external HTTP/client, file/storage và implementation của ports.

## Command/query rule

Command thay đổi state phải đi qua application use case. Query có thể dùng read model tối ưu nhưng không được làm thay đổi domain state.

## Validation

- Structural/input validation ở boundary.
- Business invariant ở domain/application.
- DB constraint là safety net, không phải nơi duy nhất định nghĩa business rule.

## Mapping

Transport DTO, application model và persistence model không mặc định là cùng một type. Mapping phải explicit tại boundary phù hợp.

## Dependency injection

Dependency external được inject qua interface/port. Domain không gọi trực tiếp clock, database, HTTP client hoặc environment configuration.

## Background work

Job/worker phải dùng cùng application/domain contracts khi thực hiện business behavior; không copy business logic sang worker implementation.

## Common references

Backend implementation đồng thời phải tuân `API-STD-001`, `TX-STD-001`, `AUTH-DES-001` và `APP-ARCH-001`.