---
documentId: DOC-REQ-INV-001
documentType: RequirementSpecification
status: Baseline
version: 1
primaryObject: REQ-INV-001
objects:
  - REQ-INV-001
  - BR-INV-001
  - AC-INV-001-01
  - AC-INV-001-02
relations:
  REQ-INV-001:
    participatesIn:
      - BF-001
      - BF-002
      - BF-003
    governedBy:
      - BR-INV-001
    acceptedBy:
      - AC-INV-001-01
      - AC-INV-001-02
    satisfiedBy:
      - DES-INV-001
---

# REQ-INV-001 — Reserve Stock

## Requirement

**ID:** `REQ-INV-001`  
**Type:** Functional Requirement

Hệ thống phải reserve tồn kho cho Order trước fulfillment và release reservation khi Order bị hủy hoặc không còn cần giữ hàng.

## Business Rule

### BR-INV-001 — No negative available stock

Một reservation chỉ được tạo khi số lượng available stock của từng item đủ đáp ứng quantity yêu cầu. Hệ thống không được cho phép available stock trở thành số âm.

## Behavior

1. Nhận yêu cầu reserve theo Order ID và order lines.
2. Kiểm tra available stock.
3. Nếu đủ, tạo reservation và giảm available stock tương ứng.
4. Nếu thiếu, trả failure mà không tạo partial reservation ngoài khi policy sau này cho phép.
5. Khi Order bị hủy, reservation phải được release idempotently.

## Acceptance Criteria

### AC-INV-001-01 — Reservation succeeds atomically

Nếu tất cả item đủ hàng, reservation được tạo đầy đủ và available stock được cập nhật nhất quán.

### AC-INV-001-02 — Insufficient stock does not oversell

Nếu bất kỳ item nào thiếu hàng, hệ thống không được làm available stock âm và phải trả kết quả thất bại có cấu trúc.

## Traceability

```text
BF-001 / BF-002 / BF-003
  ↓
REQ-INV-001
  ├── governed-by → BR-INV-001
  ├── accepted-by → AC-INV-001-01
  ├── accepted-by → AC-INV-001-02
  └── satisfied-by → DES-INV-001
```
