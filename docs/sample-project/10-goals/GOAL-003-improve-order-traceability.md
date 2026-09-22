---
id: GOAL-003
type: BusinessGoal
status: Baseline
version: 1
title: Improve order traceability
relations:
  decomposesTo:
    - BF-001
    - BF-003
---

# GOAL-003 — Improve order traceability

## Mục tiêu

Cho phép vận hành và hỗ trợ khách hàng biết một đơn hàng đang ở trạng thái nào, đã đi qua bước nào và vì sao trạng thái thay đổi.

## Business outcome

- Mọi thay đổi trạng thái đơn đều có lịch sử.
- Có thể truy ngược từ trạng thái hiện tại về các hành động hoặc event đã tạo ra trạng thái đó.
- Người dùng có thể xem tiến trình đơn trên một màn hình thống nhất.

## Phạm vi liên quan

- `BF-001` — Order to Cash.
- `BF-003` — Order Cancellation & Refund.

## Cách đo

- Tỷ lệ order status changes có audit trail.
- Số ticket hỗ trợ do không xác định được trạng thái đơn.
- Thời gian trung bình để điều tra một order incident.
