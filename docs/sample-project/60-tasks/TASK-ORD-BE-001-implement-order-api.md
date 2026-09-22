---
id: TASK-ORD-BE-001
type: Task
status: Ready
version: 1
title: Implement Order Command API
relations:
  implements:
    - API-ORD-001
    - EVT-ORD-001
  reads:
    - REQ-ORD-001
    - REQ-ORD-002
    - REQ-ORD-003
    - DES-ORD-001
    - AC-ORD-001-01
    - AC-ORD-001-02
    - BR-ORD-001
    - AC-ORD-002-01
    - AC-ORD-002-02
    - AC-ORD-003-01
    - AC-ORD-003-02
  verifiedBy:
    - TEST-ORD-001
---

# TASK-ORD-BE-001 — Implement Order Command API

## Objective

Implement `API-ORD-001` và `EVT-ORD-001` theo requirement/design baseline hiện tại.

## Mandatory read set

- `REQ-ORD-001` — Create Order.
- `REQ-ORD-002` — Approve High-value Order.
- `REQ-ORD-003` — Track Order Status.
- `DES-ORD-001` — Order Approval and Status Solution.
- Các Business Rule và Acceptance Criteria được khai báo trong ba requirement document trên.

## Write set

Task được phép tạo/sửa implementation thuộc:

- `API-ORD-001`.
- `EVT-ORD-001`.

Task không được tự thay đổi requirement hoặc introduce deliverable mới. Nếu implementation phát hiện cần thêm deliverable/contract, phải tạo change proposal.

## Expected output

- Order command endpoints.
- Approval policy handling.
- Status transition logic.
- Status history persistence.
- OrderStatusChanged event publishing.
- Unit/integration tests cần thiết.

## Done when

- `TEST-ORD-001` pass.
- Acceptance Criteria liên quan pass.
- Không có thay đổi ngoài declared scope nếu chưa có approved change.
