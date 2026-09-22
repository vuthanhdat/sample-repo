---
id: APP-ARCH-001
type: CommonDesign
scope: Project
status: Draft
version: 1
---
# APP-ARCH-001 — Application Architecture

## Purpose
Định nghĩa architecture baseline toàn application.

## Logical Structure
```text
Presentation / Delivery
        ↓
Application / Use Cases
        ↓
Domain / Business Logic
        ↓
Ports / Interfaces
        ↓
Infrastructure Adapters
```

## Rules
- Dependency hướng vào core; domain không phụ thuộc framework/DB/provider SDK.
- Feature/module boundary rõ ràng.
- Cross-feature access qua public contract, không truy cập private implementation/data mapping.
- Cross-cutting concerns dùng common pipeline/middleware/adapter phù hợp.
- Architecture exception phải có decision và impact analysis.