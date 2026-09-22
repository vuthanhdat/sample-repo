---
id: BE-ARCH-001
type: CommonDesign
scope: Backend
status: Draft
version: 1
---
# BE-ARCH-001 — Backend Design Standard

## Responsibilities
- Controller/Endpoint: transport mapping, authentication context, request validation entry point.
- Application: orchestration/use case, transaction intent, ports.
- Domain: business invariant/state transition.
- Infrastructure: persistence, messaging, external adapters.

## Rules
- Controller không chứa business decision.
- Repository không chứa use-case orchestration.
- Background job gọi application use case thay vì update business table trực tiếp.
- External DTO/SDK model không leak vào core contract.
- Dependency Injection chỉ composition; không biến service locator thành global state.