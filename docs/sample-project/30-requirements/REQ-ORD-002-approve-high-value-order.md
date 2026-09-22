---
documentId: DOC-REQ-ORD-002
documentType: RequirementSpecification
status: Baseline
version: 1
primaryObject: REQ-ORD-002
objects:
  - REQ-ORD-002
  - BR-ORD-001
  - AC-ORD-002-01
  - AC-ORD-002-02
relations:
  REQ-ORD-002:
    participatesIn:
      - BF-001
    governedBy:
      - BR-ORD-001
    acceptedBy:
      - AC-ORD-002-01
      - AC-ORD-002-02
    satisfiedBy:
      - DES-ORD-001
---

# REQ-ORD-002 — Approve High-value Order

## Requirement

**ID:** `REQ-ORD-002`  
**Type:** Functional Requirement

Hệ thống phải yêu cầu phê duyệt trước khi một Order giá trị cao được phép tiếp tục sang bước reserve tồn kho và fulfillment.

## Business Rule

### BR-ORD-001 — High-value approval threshold

Order có tổng giá trị từ `50,000,000 VND` trở lên phải được phê duyệt trước khi chuyển sang trạng thái `Approved`.

Threshold là business configuration và không được hard-code trong logic nghiệp vụ.

## Behavior

1. Sau khi Order được tạo, hệ thống đánh giá `BR-ORD-001`.
2. Nếu không vượt threshold, Order có thể tiếp tục xử lý tự động.
3. Nếu vượt threshold, Order chuyển sang `PendingApproval`.
4. Chỉ approval hợp lệ mới cho phép Order tiếp tục.
5. Rejection phải lưu reason và chuyển Order sang trạng thái phù hợp.

## Acceptance Criteria

### AC-ORD-002-01 — High-value order is blocked

Order đạt hoặc vượt threshold phải ở `PendingApproval` và chưa được reserve stock trước khi được approve.

### AC-ORD-002-02 — Approved order continues

Sau approval hợp lệ, Order phải được phép tiếp tục sang bước reserve stock đúng một lần.

## Traceability

```text
BF-001
  ↓
REQ-ORD-002
  ├── governed-by → BR-ORD-001
  ├── accepted-by → AC-ORD-002-01
  ├── accepted-by → AC-ORD-002-02
  └── satisfied-by → DES-ORD-001
```
