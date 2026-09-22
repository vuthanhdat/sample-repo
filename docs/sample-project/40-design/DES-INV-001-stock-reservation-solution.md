---
id: DES-INV-001
type: DesignDecision
status: Baseline
version: 1
title: Stock reservation solution
relations:
  satisfies:
    - REQ-INV-001
  introduces:
    - API-INV-001
---

# DES-INV-001 — Stock Reservation Solution

## Context

`REQ-INV-001` yêu cầu reserve/release stock một cách nhất quán và không cho available stock âm.

## Decision

Inventory module sẽ cung cấp một Stock Reservation API. Reservation được xử lý theo Order ID và phải idempotent để caller có thể retry an toàn.

## Deliverables introduced

- `API-INV-001` — Stock Reservation API.

## Key design rules

1. Reservation phải kiểm tra và cập nhật available stock trong transaction boundary phù hợp.
2. Không tạo partial reservation nếu policy hiện tại yêu cầu all-or-nothing.
3. Request reserve lặp lại với cùng idempotency key không được trừ tồn kho lần thứ hai.
4. Release lặp lại không được cộng tồn kho nhiều lần.
5. Reservation record phải lưu liên kết tới Order ID để audit và release.

## Traceability

```text
REQ-INV-001
    ↓ satisfied-by
DES-INV-001
    ↓ introduces
API-INV-001
```
