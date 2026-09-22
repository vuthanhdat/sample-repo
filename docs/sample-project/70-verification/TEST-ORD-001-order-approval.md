---
id: TEST-ORD-001
type: VerificationDefinition
status: Baseline
version: 1
title: Verify order creation, approval and status tracking
relations:
  verifies:
    - REQ-ORD-001
    - REQ-ORD-002
    - REQ-ORD-003
    - API-ORD-001
    - EVT-ORD-001
---

# TEST-ORD-001 — Order Creation, Approval and Status Verification

## Purpose

Xác minh behavior của Order Management đối với tạo Order, approval policy và status tracking.

## Covered objects

- `REQ-ORD-001`.
- `REQ-ORD-002`.
- `REQ-ORD-003`.
- `API-ORD-001`.
- `EVT-ORD-001`.

## Required scenarios

1. Valid standard Order được tạo thành công.
2. Invalid Order bị reject mà không persist dữ liệu không hợp lệ.
3. High-value Order chuyển sang `PendingApproval`.
4. High-value Order chưa approve không được tiếp tục reserve stock.
5. Approval hợp lệ cho phép Order tiếp tục đúng một lần.
6. Status history chứa đầy đủ previous/new status và metadata.
7. `EVT-ORD-001` được tạo cho các transition bắt buộc.

## Pass condition

Tất cả scenario bắt buộc pass trên cùng target revision của deliverable được verify.
