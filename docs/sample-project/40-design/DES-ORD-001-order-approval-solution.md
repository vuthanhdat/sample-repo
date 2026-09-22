---
id: DES-ORD-001
type: DesignDecision
status: Baseline
version: 1
title: Order approval and status solution
relations:
  satisfies:
    - REQ-ORD-001
    - REQ-ORD-002
    - REQ-ORD-003
  introduces:
    - API-ORD-001
    - SCR-ORD-001
    - EVT-ORD-001
---

# DES-ORD-001 — Order Approval and Status Solution

## Context

Các requirement `REQ-ORD-001`, `REQ-ORD-002` và `REQ-ORD-003` cần một cách thống nhất để tạo Order, áp dụng approval policy và duy trì trạng thái có thể truy vết.

## Decision

Order Management sẽ chịu ownership của Order aggregate và expose một command API để tạo/approve/cancel Order. Mọi thay đổi trạng thái quan trọng sẽ tạo domain event và được lưu trong status history. UI Order Detail đọc trạng thái hiện tại và timeline từ Order Management.

## Deliverables introduced

- `API-ORD-001` — Order Command API.
- `SCR-ORD-001` — Order Detail Screen.
- `EVT-ORD-001` — Order Status Changed Event.

## Key design rules

1. Order state transition phải được validate ở domain/application layer.
2. Approval threshold phải đọc từ configuration/policy, không hard-code.
3. Status history phải được ghi cùng transaction logic của transition hoặc bằng cơ chế đảm bảo eventual consistency có kiểm soát.
4. Command phải idempotent tại các boundary có nguy cơ retry.
5. Event payload phải chứa Order ID, previous status, new status, occurred time và causation metadata tối thiểu.

## Traceability

```text
REQ-ORD-001 ─┐
REQ-ORD-002 ─┼─ satisfied-by → DES-ORD-001
REQ-ORD-003 ─┘
                         ├─ introduces → API-ORD-001
                         ├─ introduces → SCR-ORD-001
                         └─ introduces → EVT-ORD-001
```
