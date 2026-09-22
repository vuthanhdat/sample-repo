---
documentId: DOC-REQ-ORD-001
documentType: RequirementSpecification
status: Baseline
version: 1
primaryObject: REQ-ORD-001
objects:
  - REQ-ORD-001
  - AC-ORD-001-01
  - AC-ORD-001-02
relations:
  REQ-ORD-001:
    participatesIn:
      - BF-001
    acceptedBy:
      - AC-ORD-001-01
      - AC-ORD-001-02
    satisfiedBy:
      - DES-ORD-001
---

# REQ-ORD-001 — Create Order

## Requirement

**ID:** `REQ-ORD-001`  
**Type:** Functional Requirement

Hệ thống phải cho phép tạo một Order từ thông tin khách hàng và danh sách sản phẩm, tính tổng giá trị đơn và gán trạng thái ban đầu phù hợp để bắt đầu xử lý.

## Input

- Customer ID.
- Order lines: Product ID, quantity, unit price.
- Shipping information.

## Output

- Order ID.
- Calculated total amount.
- Initial order status.

## Business behavior

1. Validate dữ liệu bắt buộc.
2. Tính tổng giá trị đơn từ các order line.
3. Tạo Order với ID duy nhất.
4. Chuyển sang bước kiểm tra approval policy.

## Acceptance Criteria

### AC-ORD-001-01 — Valid order is created

Khi request hợp lệ, hệ thống phải tạo đúng một Order và trả về Order ID.

### AC-ORD-001-02 — Invalid order is rejected

Khi thiếu customer hoặc không có order line hợp lệ, hệ thống không được tạo Order và phải trả lỗi validation có cấu trúc.

## Traceability

```text
BF-001
  ↓
REQ-ORD-001
  ├── accepted-by → AC-ORD-001-01
  ├── accepted-by → AC-ORD-001-02
  └── satisfied-by → DES-ORD-001
```
