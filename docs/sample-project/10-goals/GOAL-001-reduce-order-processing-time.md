---
id: GOAL-001
type: BusinessGoal
status: Baseline
version: 1
title: Reduce order processing time
relations:
  decomposesTo:
    - BF-001
    - BF-003
---

# GOAL-001 — Reduce order processing time

## Mục tiêu

Giảm thời gian trung bình từ lúc khách hàng tạo đơn đến lúc đơn đủ điều kiện để fulfillment bắt đầu.

## Business outcome

- Thời gian xử lý đơn tiêu chuẩn dưới 5 phút trong điều kiện không cần phê duyệt thủ công.
- Đơn giá trị cao phải được phát hiện và chuyển sang luồng phê duyệt mà không cần thao tác thủ công từ bộ phận vận hành.
- Hạn chế việc đơn phải xử lý lại do thiếu kiểm tra tồn kho hoặc sai trạng thái.

## Phạm vi liên quan

Goal này được hiện thực thông qua:

- `BF-001` — Order to Cash.
- `BF-003` — Order Cancellation & Refund.

## Cách đo

- Median order-ready time.
- P95 order-ready time.
- Tỷ lệ đơn cần xử lý lại.
- Tỷ lệ đơn bị treo do thiếu trạng thái hoặc thiếu phê duyệt.
