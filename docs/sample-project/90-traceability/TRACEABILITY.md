# Sample Project Traceability View

File này là **derived view** để con người kiểm tra nhanh traceability. Trong hệ thống thật, matrix này nên được generate từ relation graph thay vì nhập tay làm source of truth.

## Goal → Flow

| Goal | Business Flow |
|---|---|
| `GOAL-001` | `BF-001`, `BF-003` |
| `GOAL-002` | `BF-001`, `BF-002` |
| `GOAL-003` | `BF-001`, `BF-003` |

## Flow → Requirement

| Business Flow | Requirements |
|---|---|
| `BF-001` | `REQ-ORD-001`, `REQ-ORD-002`, `REQ-INV-001`, `REQ-ORD-003` |
| `BF-002` | `REQ-INV-001` |
| `BF-003` | `REQ-INV-001`, `REQ-ORD-003` |

## Requirement → Design → Deliverable

| Requirement | Design | Deliverables |
|---|---|---|
| `REQ-ORD-001` | `DES-ORD-001` | `API-ORD-001` |
| `REQ-ORD-002` | `DES-ORD-001` | `API-ORD-001` |
| `REQ-ORD-003` | `DES-ORD-001` | `API-ORD-001`, `SCR-ORD-001`, `EVT-ORD-001` |
| `REQ-INV-001` | `DES-INV-001` | `API-INV-001` |

## Deliverable → Task → Verification

| Deliverable | Implementing Task | Verification |
|---|---|---|
| `API-ORD-001` | `TASK-ORD-BE-001` | `TEST-ORD-001` |
| `EVT-ORD-001` | `TASK-ORD-BE-001` | `TEST-ORD-001` |
| `SCR-ORD-001` | `TASK-ORD-FE-001` | E2E through `TASK-E2E-001` |
| `API-INV-001` | `TASK-INV-BE-001` | `TEST-INV-001` |

## Example full path

```text
GOAL-001
  ↓ decomposes-to
BF-001
  ↓ decomposes-to
REQ-ORD-002
  ├── governed-by → BR-ORD-001
  ├── accepted-by → AC-ORD-002-01
  ├── accepted-by → AC-ORD-002-02
  ↓ satisfied-by
DES-ORD-001
  ↓ introduces
API-ORD-001
  ↓ implemented-by
TASK-ORD-BE-001
  ↓ verified-by
TEST-ORD-001
```

## Example impact analysis

Nếu `BR-ORD-001` thay đổi approval threshold hoặc logic phân loại high-value order, graph cho thấy tối thiểu các object cần được đánh giá impact:

```text
BR-ORD-001
  ↓ governs
REQ-ORD-002
  ↓ satisfied-by
DES-ORD-001
  ↓ introduces
API-ORD-001
  ↓ implemented-by
TASK-ORD-BE-001
  ↓ verified-by
TEST-ORD-001
```

Không có nghĩa tất cả object bắt buộc phải sửa; hệ thống chỉ xác định **candidate impacted objects**, sau đó từng object được disposition thành `Update Required`, `Review Required`, `Revalidation Required` hoặc `No Change Required`.
