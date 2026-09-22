---
id: GOAL-002
type: BusinessGoal
status: Baseline
version: 1
title: Improve inventory accuracy
relations:
  decomposesTo:
    - BF-001
    - BF-002
---

# GOAL-002 — Improve inventory accuracy

## Mục tiêu

Đảm bảo số lượng tồn kho khả dụng mà hệ thống sử dụng để nhận đơn phản ánh đúng lượng hàng có thể cam kết cho khách hàng.

## Business outcome

- Không tạo đơn vượt quá tồn kho khả dụng.
- Việc reserve/release tồn kho phải có lịch sử truy vết.
- Hạn chế chênh lệch giữa tồn kho nghiệp vụ và tồn kho thực tế.

## Phạm vi liên quan

- `BF-001` — Order to Cash.
- `BF-002` — Inventory Replenishment.

## Cách đo

- Tỷ lệ oversell.
- Tỷ lệ reservation lỗi.
- Chênh lệch giữa available stock và physical stock.
- Số incident do tồn kho âm.
