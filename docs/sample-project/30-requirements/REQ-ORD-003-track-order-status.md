---
documentId: DOC-REQ-ORD-003
documentType: RequirementSpecification
status: Baseline
version: 1
primaryObject: REQ-ORD-003
objects:
  - REQ-ORD-003
  - AC-ORD-003-01
  - AC-ORD-003-02
relations:
  REQ-ORD-003:
    participatesIn:
      - BF-001
      - BF-003
    acceptedBy:
      - AC-ORD-003-01
      - AC-ORD-003-02
    satisfiedBy:
      - DES-ORD-001
---

# REQ-ORD-003 — Track Order Status

## Requirement

**ID:** `REQ-ORD-003`  
**Type:** Functional Requirement

Hệ thống phải lưu và hiển thị trạng thái hiện tại của Order cùng lịch sử các lần chuyển trạng thái để người dùng có thể truy vết tiến trình xử lý.

## Behavior

1. Mỗi status transition phải ghi trạng thái trước, trạng thái sau, thời điểm và actor/source gây thay đổi.
2. Trạng thái hiện tại phải phản ánh transition mới nhất hợp lệ.
3. Người dùng phải xem được timeline trạng thái trên Order Detail.
4. Các hệ thống tích hợp có thể nhận thông báo khi trạng thái quan trọng thay đổi.

## Acceptance Criteria

### AC-ORD-003-01 — Status history is complete

Mọi status transition hợp lệ đều tạo một history record với source và timestamp.

### AC-ORD-003-02 — Current status is visible

Order Detail phải hiển thị trạng thái hiện tại và timeline theo thứ tự thời gian.

## Traceability

```text
BF-001 / BF-003
  ↓
REQ-ORD-003
  ├── accepted-by → AC-ORD-003-01
  ├── accepted-by → AC-ORD-003-02
  └── satisfied-by → DES-ORD-001
```
