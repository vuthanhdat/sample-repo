---
id: DES-ORD-001
type: DesignDecision
status: Baseline
version: 2
title: Order approval and status solution
relations:
  satisfies:
    - REQ-ORD-001
    - REQ-ORD-002
    - REQ-ORD-003
  constrainedBy:
    - NFR-PERF-001
    - NFR-REL-001
    - SREQ-001
    - DREQ-001
  introduces:
    - API-ORD-001
    - SCR-ORD-001
    - EVT-ORD-001
    - DB-ORD-001
---

# DES-ORD-001 — Order Approval and Status Solution

## Context

Các requirement `REQ-ORD-001`, `REQ-ORD-002` và `REQ-ORD-003` cần một cách thống nhất để tạo Order, áp dụng approval policy và duy trì trạng thái có thể truy vết. Solution cũng phải tuân performance, reliability, security và data requirements của project.

## Decision

Order Management chịu ownership của Order aggregate. Business actions Create/Approve/Cancel đi qua explicit application commands/API; state transition được validate trong domain/application boundary. Mỗi transition quan trọng tạo business history và event. Order data được persist trong database deliverable do Order module sở hữu; UI chỉ consume API contract và không tự quyết định business state.

## Deliverables introduced

- `API-ORD-001` — Order Command API.
- `SCR-ORD-001` — Order Detail Screen.
- `EVT-ORD-001` — Order Status Changed Event.
- `DB-ORD-001` — Order Management Database.

## Detailed specifications

- `API-DES-001` specifies `API-ORD-001`.
- `SCR-DES-001` specifies `SCR-ORD-001`.
- `EVT-DES-001` specifies `EVT-ORD-001`.
- `DBD-001`, `DBD-002` specify `DB-ORD-001`.
- `SEC-DES-001` applies authorization/audit policy.
- `CFG-DES-001` defines changeable business configuration such as approval threshold.

## Key design rules

1. Order state transition phải được validate ở domain/application layer.
2. Approval threshold đọc từ governed configuration/policy, không hard-code.
3. Status history được persist cùng canonical transition boundary hoặc bằng consistency mechanism đã thiết kế rõ.
4. Commands có side effect phải có idempotency/concurrency semantics phù hợp.
5. Event publish tuân `EVT-DES-001`; không phát event cho uncommitted state.
6. API/UI/database artifacts không tự copy business rule thành nguồn sự thật khác.

## Traceability

```text
REQ-ORD-* + NFR/DREQ/SREQ
        ↓ constrained/satisfied-by
DES-ORD-001
   ├─ introduces → API-ORD-001 ← specifies API-DES-001
   ├─ introduces → SCR-ORD-001 ← specifies SCR-DES-001
   ├─ introduces → EVT-ORD-001 ← specifies EVT-DES-001
   └─ introduces → DB-ORD-001  ← specifies DBD-001/DBD-002
```
