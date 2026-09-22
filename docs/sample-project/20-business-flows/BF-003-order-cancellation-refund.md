---
id: BF-003
type: BusinessFlow
status: Baseline
version: 1
title: Order Cancellation and Refund
relations:
  contributesTo:
    - GOAL-001
    - GOAL-003
  decomposesTo:
    - REQ-ORD-003
    - REQ-INV-001
---

# BF-003 — Order Cancellation and Refund

## Mục đích

Mô tả luồng khi một đơn bị hủy trước fulfillment hoặc bị dừng do điều kiện nghiệp vụ không còn hợp lệ.

## Actors

- Customer
- Operations Staff
- Order Management System
- Inventory System

## Trigger

Khách hàng hoặc nhân viên vận hành yêu cầu hủy đơn, hoặc hệ thống xác định đơn không thể tiếp tục xử lý.

## Main flow

1. Hệ thống kiểm tra trạng thái hiện tại của đơn.
2. Nếu đơn còn được phép hủy, trạng thái chuyển sang `CancellationRequested`.
3. Reservation tồn kho liên quan được release.
4. Đơn chuyển sang `Cancelled` sau khi các xử lý liên quan hoàn tất.
5. Lịch sử trạng thái và lý do hủy được lưu để truy vết.

## Output

- Order cancellation state.
- Released stock reservation.
- Status/audit history.

## Requirements

- `REQ-ORD-003` — Track Order Status.
- `REQ-INV-001` — Reserve/Release Stock.
