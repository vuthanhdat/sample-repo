---
id: APP-ARCH-001
type: CommonDesign
scope: Common
status: Baseline
version: 1
title: Application Architecture
appliesTo:
  - Project
---

# APP-ARCH-001 — Application Architecture

## Purpose

Định nghĩa kiến trúc kỹ thuật chung cho toàn application. Các business module như Order và Inventory phải tuân theo baseline này.

## Logical layers

```text
Presentation / Delivery
        ↓
Application / Use Cases
        ↓
Domain
        ↓
Ports / Interfaces
        ↓
Infrastructure Adapters
```

Dependency chỉ đi vào phía domain/application. Domain không phụ thuộc web framework, database driver hoặc external SDK.

## Module rule

- Mỗi business capability lớn có module boundary rõ ràng.
- Module chỉ truy cập data của module khác qua public contract/application boundary đã định nghĩa.
- Shared technical utilities không được trở thành nơi chứa business logic.
- Cross-cutting concerns đi qua middleware/pipeline/decorator hoặc shared infrastructure có contract rõ ràng.

## Runtime components

- Web/API host.
- Background worker host khi có Job.
- Relational database.
- External adapters.
- Observability pipeline.

## Cross-cutting baseline

Toàn application dùng chung rule cho authentication, authorization, validation, error handling, transaction, concurrency, pagination, logging/audit, configuration và observability. Chi tiết nằm trong các Common Design documents khác.

## Exception rule

Feature muốn phá vỡ dependency direction hoặc common baseline phải có design decision/ADR riêng và impact analysis.