---
id: TASK-E2E-001
type: Task
status: Blocked
version: 1
title: Implement Order Flow E2E Test
relations:
  reads:
    - BF-001
    - REQ-ORD-001
    - REQ-ORD-002
    - REQ-INV-001
    - REQ-ORD-003
    - API-ORD-001
    - API-INV-001
    - SCR-ORD-001
  dependsOn:
    - TASK-ORD-BE-001
    - TASK-INV-BE-001
    - TASK-ORD-FE-001
  verifies:
    - GOAL-001
    - BF-001
---

# TASK-E2E-001 — Implement Order Flow E2E Test

## Objective

Tạo automated E2E scenario cho luồng chính của `BF-001` từ tạo Order đến approval, stock reservation và hiển thị trạng thái.

## Mandatory read set

- `BF-001`.
- Các requirement `REQ-ORD-001`, `REQ-ORD-002`, `REQ-INV-001`, `REQ-ORD-003`.
- Các deliverable contract `API-ORD-001`, `API-INV-001`, `SCR-ORD-001`.

## Dependencies

- `TASK-ORD-BE-001`.
- `TASK-INV-BE-001`.
- `TASK-ORD-FE-001`.

## Scenario

1. Tạo Order giá trị cao.
2. Xác nhận Order ở `PendingApproval`.
3. Approve Order.
4. Xác nhận stock được reserve.
5. Xác nhận Order chuyển sang trạng thái sẵn sàng tiếp tục xử lý.
6. Mở Order Detail và xác nhận timeline trạng thái đầy đủ.

## Done when

- E2E chạy repeatable trên test environment.
- Scenario chính pass.
- Failure output chỉ rõ step/object ID liên quan.
