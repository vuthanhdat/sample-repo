---
id: SCR-DES-001
type: DesignSpecification
subtype: Screen
status: Baseline
version: 1
title: Order Detail screen specification
relations:
  specifies:
    - SCR-ORD-001
  satisfies:
    - REQ-ORD-003
  uses:
    - API-ORD-001
  constrainedBy:
    - SREQ-001
    - NFR-PERF-001
---

# SCR-DES-001 — Order Detail Screen

## Purpose

Cho Operations/authorized user xem toàn bộ trạng thái Order và thực hiện action phù hợp mà không phải ghép thông tin thủ công từ nhiều màn hình.

## Layout

1. **Header**: Order No, customer, current status, total amount, created time.
2. **Order lines**: product, quantity, unit price, line amount.
3. **Approval panel**: required/not required, current decision, actor/time.
4. **Inventory panel**: reservation status summary.
5. **Status timeline**: paged history newest/oldest option.
6. **Actions**: Approve, Reject/Cancel tùy state + permission.

## UI action rules

Action visibility chỉ là UX convenience. Backend `API-ORD-001` vẫn authorize/validate độc lập. UI phải disable duplicate submission trong lúc command đang pending và hỗ trợ retry có kiểm soát khi response không xác định.

## State examples

| Order state | Main actions |
|---|---|
| Draft | Submit/Cancel nếu có quyền |
| PendingApproval | Approve/Reject cho approver hợp lệ |
| Approved | Không hiển thị Approve lại |
| ReadyForFulfillment | Cancel chỉ nếu policy cho phép |
| Cancelled | Read-only |

## Error behavior

- `CONCURRENCY_CONFLICT`: refresh data, thông báo state đã thay đổi.
- `FORBIDDEN`: không giả định UI permission cache còn đúng.
- `DEPENDENCY_UNAVAILABLE`: hiển thị retryable message nhưng không tự đoán command đã fail/succeed nếu outcome unknown.

## Accessibility/usability baseline

Action quan trọng có label rõ, keyboard accessible; status không chỉ phân biệt bằng màu; bảng/timeline hỗ trợ loading/empty/error states.
