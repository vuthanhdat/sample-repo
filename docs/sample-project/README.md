# Sample Project — Order & Inventory Management

Đây là bộ tài liệu mẫu cho một software project được quản lý bằng ID, relation, version, lifecycle và traceability. Folder chỉ là navigation; graph giữa Goal → Requirement → Design → Deliverable → Task → Verification mới là model logic.

## Cấu trúc chính

```text
docs/sample-project/
├── 00-governance/
├── 10-goals/
├── 15-business-context/
├── 20-business-flows/
├── 30-requirements/
├── 40-design/
│   ├── 10-common/
│   │   ├── architecture/
│   │   ├── backend/
│   │   ├── frontend/
│   │   ├── api/
│   │   └── security/
│   └── 20-business/
│       ├── 00-shared-domain/
│       │   ├── architecture/
│       │   ├── data/
│       │   └── configuration/
│       ├── order/
│       │   ├── decision/
│       │   ├── api/
│       │   ├── screen/
│       │   └── event/
│       ├── inventory/
│       │   ├── decision/
│       │   ├── api/
│       │   └── job/
│       └── shipping/
│           └── integration/
├── 50-deliverables/
│   ├── 00-lists/
│   └── ... deliverable specifications ...
├── 55-planning/
├── 60-tasks/
├── 70-verification/
├── 80-operations/
├── 85-change-management/
└── 90-traceability/
```

## Design có hai scope độc lập

### Common / Application Design

`40-design/10-common/` trả lời: **application được xây như thế nào bất kể business feature là gì?**

- `APP-ARCH-001`: application architecture và dependency direction.
- `BE-ARCH-001`: backend responsibility/layering.
- `FE-ARCH-001`: frontend shell/routing/state/data/form conventions.
- `API-STD-001`: REST/API naming, pagination, filtering/sorting, error contract, idempotency.
- `TX-STD-001`: transaction boundary, concurrency, retry.
- `AUTH-DES-001`: login/logout/session/token lifecycle.
- `SEC-DES-001`: authorization enforcement và audit model.

Common Design là baseline có version. Business design không copy lại rule chung; nó kế thừa hoặc khai báo exception có trace.

### Business / Feature Design

`40-design/20-business/` trả lời: **solution cho business requirement/domain cụ thể phải trông như thế nào?**

Nhánh này tổ chức theo domain trước, artifact type sau:

- `00-shared-domain`: architecture/data/config có business meaning nhưng span nhiều domain.
- `order`: Order decision, API, screen, event.
- `inventory`: Inventory decision, API, job.
- `shipping`: shipping interface/integration.

Ví dụ:

```text
API-STD-001 (Common)
       ↓ applies-to
API-DES-001 (Order business design)
       ↓ specifies
API-ORD-001
       ↓ implemented-by
TASK-ORD-BE-001
```

## Tại sao không dùng `40-design/api`, `40-design/screen`, `40-design/job` ở root?

Vì cách đó trộn hai dimension khác nhau: scope và artifact type. Nó làm API common convention đứng cạnh API nghiệp vụ, đồng thời làm knowledge của một feature bị rải ở nhiều folder. Cấu trúc mới dùng:

```text
scope → domain → artifact type
```

thay vì:

```text
artifact type → mọi scope/domain trộn chung
```

App vẫn có thể tạo view “all APIs”, “all screens”, “all jobs” từ metadata, không cần dùng folder design làm inventory.

## Output inventory lists

`50-deliverables/00-lists/` là management view theo loại output:

- `SCREEN-LIST.md`
- `API-LIST.md`
- `BATCH-JOB-LIST.md`
- `INTERFACE-LIST.md`
- `EVENT-LIST.md`
- `DATABASE-LIST.md`
- `FILE-LIST.md`
- `MAIL-LIST.md`
- `NOTIFICATION-LIST.md`

List không thay detailed design. Ví dụ `SCREEN-LIST` quản lý toàn bộ screen và coverage; `SCR-DES-001` mới là detailed design của một screen. Trong SaaS thật, các list nên generate từ Deliverable Registry.

## Task context

Human hoặc AI nhận Task ID phải được app resolve hai loại context:

```text
Inherited Common Baseline
  +
Explicit Business Context
  +
Target Deliverables
  +
Verification Definitions
```

Ví dụ backend Inventory task kế thừa `APP-ARCH-001`, `BE-ARCH-001`, `API-STD-001`, `TX-STD-001`, `AUTH-DES-001`; đồng thời đọc explicit `REQ-INV-001`, `DES-INV-001`, `API-DES-002`, `DBD-*`.

## Change impact

Common Design thay đổi có blast radius rộng. Business Design thay đổi thường có scope hẹp hơn theo domain. SaaS phải dùng relation graph để xác định candidate impacts thay vì suy luận từ folder.

```text
Common Standard change
    ↓
Affected Business Designs
    ↓
Affected Deliverables
    ↓
Tasks / Tests / Operations
```

## Nguyên tắc cuối

Folder phản ánh cách con người duyệt knowledge. ID/relation phản ánh model thật. ProjectTemplate phải quản lý applicability, required document products, common baseline, business design coverage, deliverable inventory, task coverage và verification coverage thay vì chỉ kiểm tra sự tồn tại của Markdown file.