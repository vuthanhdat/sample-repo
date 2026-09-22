# Sample Project — Order & Inventory Management

Đây là một **cây tài liệu mẫu của một project thật**, dùng để kiểm tra mô hình document/traceability của Software Project Governance SaaS.

Mỗi file đại diện cho một object hoặc một design/output có ID ổn định. Quan hệ giữa các object được khai báo trong front matter và cũng được diễn giải trong nội dung để con người đọc được.

## Cấu trúc

```text
docs/sample-project/
├── README.md
├── 10-goals/
│   ├── GOAL-001-reduce-order-processing-time.md
│   ├── GOAL-002-improve-inventory-accuracy.md
│   └── GOAL-003-improve-order-traceability.md
├── 20-business-flows/
│   ├── BF-001-order-to-cash.md
│   ├── BF-002-inventory-replenishment.md
│   └── BF-003-order-cancellation-refund.md
├── 30-requirements/
│   ├── REQ-ORD-001-create-order.md
│   ├── REQ-ORD-002-approve-high-value-order.md
│   ├── REQ-INV-001-reserve-stock.md
│   └── REQ-ORD-003-track-order-status.md
├── 40-design/
│   ├── DES-ORD-001-order-approval-solution.md
│   └── DES-INV-001-stock-reservation-solution.md
├── 50-deliverables/
│   ├── API-ORD-001-order-command-api.md
│   ├── API-INV-001-stock-reservation-api.md
│   ├── EVT-ORD-001-order-status-changed.md
│   └── SCR-ORD-001-order-detail.md
├── 60-tasks/
│   ├── TASK-ORD-BE-001-implement-order-api.md
│   ├── TASK-INV-BE-001-implement-stock-reservation.md
│   ├── TASK-ORD-FE-001-implement-order-detail.md
│   └── TASK-E2E-001-order-flow-test.md
└── 70-verification/
    ├── TEST-ORD-001-order-approval.md
    └── TEST-INV-001-stock-reservation.md
```

## Drill-down chính

```text
GOAL-001
  ↓
BF-001
  ↓
REQ-ORD-001 / REQ-ORD-002 / REQ-INV-001
  ↓
DES-ORD-001 / DES-INV-001
  ↓
API-ORD-001 / API-INV-001 / SCR-ORD-001 / EVT-ORD-001
  ↓
TASK-ORD-BE-001 / TASK-INV-BE-001 / TASK-ORD-FE-001 / TASK-E2E-001
  ↓
TEST-ORD-001 / TEST-INV-001
```

## Quy tắc của sample

1. Folder thể hiện **knowledge/document layer**, không phải dependency graph.
2. ID trong front matter là identity chính của object; filename chỉ là projection dễ đọc.
3. Quan hệ được khai báo bằng ID, không suy luận từ vị trí folder.
4. Requirement có thể tham gia nhiều Business Flow.
5. Một Design Decision có thể introduce nhiều Deliverable.
6. Một Deliverable có thể được implement bởi nhiều Task.
7. Task chỉ được phép thay đổi các deliverable nằm trong `writeSet`.
8. Khi một object baseline thay đổi, impact analysis đi theo relation graph chứ không chỉ theo cây folder.
