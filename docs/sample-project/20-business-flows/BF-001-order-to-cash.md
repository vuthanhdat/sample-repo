---
id: BF-001
type: BusinessFlow
status: Baseline
version: 1
title: Order to Cash
relations:
  contributesTo:
    - GOAL-001
    - GOAL-002
    - GOAL-003
  decomposesTo:
    - REQ-ORD-001
    - REQ-ORD-002
    - REQ-INV-001
    - REQ-ORD-003
---

# BF-001 — Order to Cash

## Mục đích

Mô tả luồng từ khi khách hàng tạo đơn đến khi đơn đủ điều kiện fulfillment và trạng thái có thể được theo dõi.

## Actors

- Customer
- Sales/Operations Staff
- Order Management System
- Inventory System

## Trigger

Khách hàng xác nhận giỏ hàng và gửi yêu cầu đặt hàng.

## Main flow

1. Hệ thống tiếp nhận thông tin đơn hàng.
2. Hệ thống kiểm tra dữ liệu bắt buộc và tính tổng giá trị đơn.
3. Hệ thống kiểm tra quy tắc phê duyệt.
4. Nếu cần, đơn chuyển sang trạng thái `PendingApproval`.
5. Sau khi đủ điều kiện, hệ thống yêu cầu reserve tồn kho.
6. Khi reserve thành công, đơn chuyển sang trạng thái `ReadyForFulfillment`.
7. Mọi thay đổi trạng thái được ghi nhận để truy vết.

## Output

- Order được tạo.
- Approval decision nếu cần.
- Stock reservation.
- Order status history.

## Requirements

- `REQ-ORD-001` — Create Order.
- `REQ-ORD-002` — Approve High-value Order.
- `REQ-INV-001` — Reserve Stock.
- `REQ-ORD-003` — Track Order Status.
