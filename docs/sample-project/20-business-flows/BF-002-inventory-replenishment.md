---
id: BF-002
type: BusinessFlow
status: Baseline
version: 1
title: Inventory Replenishment
relations:
  contributesTo:
    - GOAL-002
  decomposesTo:
    - REQ-INV-001
---

# BF-002 — Inventory Replenishment

## Mục đích

Mô tả cách tồn kho được bổ sung và trở thành lượng hàng có thể cam kết cho đơn bán.

## Actors

- Warehouse Staff
- Inventory System
- Procurement/Supply Process

## Trigger

Hàng được nhập kho hoặc một giao dịch điều chỉnh tồn kho được xác nhận.

## Main flow

1. Hệ thống nhận thông tin hàng nhập hoặc điều chỉnh.
2. Số lượng physical/on-hand stock được cập nhật.
3. Hệ thống tính lại available stock sau khi trừ các reservation đang tồn tại.
4. Available stock mới được dùng cho các yêu cầu reserve tiếp theo.
5. Mọi thay đổi tồn kho được ghi audit trail.

## Output

- Updated on-hand quantity.
- Updated available quantity.
- Inventory transaction history.

## Requirements

- `REQ-INV-001` — Reserve Stock.
