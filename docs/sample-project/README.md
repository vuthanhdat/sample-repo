# Sample Project — Order & Inventory Management

Đây là một **bộ tài liệu mẫu có cấu trúc của một software project**, dùng để kiểm tra mô hình document/template/traceability mà SaaS sẽ quản lý. Điểm quan trọng của version này là tách rõ **Common/Application Design** khỏi **Business/Feature Design**, đồng thời có các **output inventory lists** để quản lý màn hình, API, batch/job, interface, event, database, file, mail và notification.

## 1. Cấu trúc

```text
docs/sample-project/
├── README.md
├── 00-governance/
│   └── DOCUMENT-CATALOG.md
├── 10-goals/
├── 15-business-context/
├── 20-business-flows/
├── 30-requirements/
│   ├── REQ-*.md
│   ├── non-functional/
│   ├── data/
│   ├── integration/
│   └── security/
│
├── 40-design/
│   ├── README.md
│   ├── 00-common/
│   │   ├── architecture/
│   │   │   └── APP-ARCH-001-application-architecture.md
│   │   ├── backend/
│   │   │   └── BE-ARCH-001-backend-design.md
│   │   ├── frontend/
│   │   │   └── FE-ARCH-001-frontend-design.md
│   │   ├── api/
│   │   │   └── API-STD-001-api-conventions.md
│   │   ├── data/
│   │   │   └── TX-STD-001-transaction-concurrency.md
│   │   └── security/
│   │       └── AUTH-DES-001-authentication-session.md
│   │
│   ├── DES-ORD-001-order-approval-solution.md
│   ├── DES-INV-001-stock-reservation-solution.md
│   ├── architecture/
│   ├── data/
│   ├── api/
│   ├── screen/
│   ├── job/
│   ├── event/
│   ├── integration/
│   ├── security/
│   └── configuration/
│
├── 50-deliverables/
│   ├── 00-lists/
│   │   ├── README.md
│   │   ├── SCREEN-LIST.md
│   │   ├── API-LIST.md
│   │   ├── BATCH-JOB-LIST.md
│   │   ├── INTERFACE-LIST.md
│   │   ├── EVENT-LIST.md
│   │   ├── DATABASE-LIST.md
│   │   ├── FILE-LIST.md
│   │   ├── MAIL-LIST.md
│   │   └── NOTIFICATION-LIST.md
│   ├── API-ORD-001-order-command-api.md
│   ├── API-INV-001-stock-reservation-api.md
│   ├── SCR-ORD-001-order-detail.md
│   ├── EVT-ORD-001-order-status-changed.md
│   ├── DB-ORD-001-order-database.md
│   ├── DB-INV-001-inventory-database.md
│   ├── JOB-INV-001-release-expired-reservations.md
│   └── INT-SHP-001-shipping-interface.md
├── 55-planning/
├── 60-tasks/
├── 70-verification/
├── 80-operations/
├── 85-change-management/
└── 90-traceability/
```

## 2. Hai lớp Design khác nhau

### 2.1 Common / Application Design

Common Design trả lời câu hỏi: **toàn application được xây như thế nào bất kể nghiệp vụ cụ thể là gì?**

Ví dụ:

- `APP-ARCH-001` — kiến trúc application, dependency direction, module boundary.
- `BE-ARCH-001` — responsibility của Controller/Application/Domain/Infrastructure.
- `FE-ARCH-001` — app shell, routing, state/data access, form, i18n, accessibility.
- `API-STD-001` — API conventions, pagination, sort/filter, error model, idempotency.
- `TX-STD-001` — transaction boundary, external I/O, concurrency, retry, job transaction.
- `AUTH-DES-001` — login, logout, session/token lifecycle, authentication vs authorization.

Các tài liệu này là **baseline được kế thừa** bởi nhiều feature. Business design không lặp lại các rule chung trừ khi cần override có kiểm soát.

```text
Common Design Baseline
      ↓ applies-to
Business / Feature Design
      ↓ specifies
Deliverable
      ↓ implemented-by
Task
```

### 2.2 Business / Feature Design

Business Design trả lời câu hỏi: **để đáp ứng requirement nghiệp vụ cụ thể thì solution này phải trông như thế nào?**

Ví dụ:

- `DES-ORD-001` — solution cho Order approval/status.
- `DES-INV-001` — solution cho stock reservation.
- `DBD-*` — data model/schema của Order/Inventory.
- `API-DES-*` — API contract nghiệp vụ.
- `SCR-DES-*` — screen cụ thể.
- `JOB-DES-*` — job cụ thể.
- `EVT-DES-*` — event cụ thể.
- `INT-DES-*` — external interface cụ thể.
- `CFG-DES-*` — business configuration cụ thể.

Ví dụ `API-DES-001` không cần định nghĩa lại pagination/error format; nó kế thừa `API-STD-001`.

## 3. Common Design cũng phải trace và version

Common Design không phải tài liệu tham khảo vô thưởng vô phạt. Nó là object có ID/version/baseline. Nếu `TX-STD-001` thay đổi transaction strategy hoặc `API-STD-001` thay đổi pagination contract, hệ thống phải tìm các design/deliverable/task bị ảnh hưởng theo `appliesTo`.

Mỗi common design object nên có tối thiểu:

```text
id
scope = Common
status/version
appliesTo
relations
```

Business design có thể khai báo explicit exception nếu không tuân baseline.

## 4. Output Inventory Lists

Design spec mô tả **một output cụ thể**. Inventory list quản lý **toàn bộ output cùng loại**.

Ví dụ:

```text
SCREEN-LIST.md
   ├── SCR-ORD-001
   ├── SCR-ORD-002
   └── ...

SCR-ORD-001
   ↑ specified-by
SCR-DES-001
```

Các list hiện có:

| List | Quản lý |
|---|---|
| `SCREEN-LIST.md` | màn hình/page |
| `API-LIST.md` | API/service contract |
| `BATCH-JOB-LIST.md` | batch, scheduler, background job |
| `INTERFACE-LIST.md` | interface giữa system |
| `EVENT-LIST.md` | event/message |
| `DATABASE-LIST.md` | database/schema/data store |
| `FILE-LIST.md` | file import/export |
| `MAIL-LIST.md` | business email |
| `NOTIFICATION-LIST.md` | in-app/push/system notification |

Mỗi row không chỉ có ID/name mà phải link được về requirement, detailed design, task, verification và lifecycle status.

Trong SaaS thật, các list này nên là **generated views từ Deliverable registry**, không phải nhiều bảng Markdown được nhập tay.

## 5. Ví dụ: pagination nằm ở đâu?

Pagination là common behavior nên nằm ở `API-STD-001` và `FE-ARCH-001`, không lặp trong từng screen/API.

```text
API-STD-001
  defines page/pageSize/sort/filter/response metadata
       ↓
API-DES-xxx
       ↓
SCR-DES-xxx / frontend list page
```

Nếu một API có dataset đặc biệt cần cursor pagination, API design đó ghi exception rõ ràng.

## 6. Ví dụ: transaction nằm ở đâu?

Transaction/concurrency baseline nằm ở `TX-STD-001`:

- một use case write phải có transaction boundary rõ;
- tránh giữ DB transaction khi gọi external service;
- define optimistic/pessimistic concurrency;
- retry chỉ cho transient error và phải idempotent;
- job define chunk/unit-of-work boundary.

`DES-INV-001` hoặc `JOB-DES-001` chỉ mô tả phần transaction đặc thù của stock reservation/expiry job.

## 7. Ví dụ: login/logout nằm ở đâu?

Login/logout là application-level feature/cross-cutting capability nên nằm ở `AUTH-DES-001`, không thuộc Order hay Inventory.

Business screen/API chỉ khai báo policy cần thiết như:

```text
Order.View
Order.Approve
Inventory.Reserve
```

Authentication trả lời user là ai; authorization quyết định action nào được phép.

## 8. Deliverable inventory và coverage

ProjectTemplate không chỉ định folder/file; nó phải biết **deliverable types** và list nào cần quản lý. Ví dụ một project có thể đánh giá:

```text
Screen        Applicable
API           Applicable
Batch/Job     Applicable
Interface     Applicable
Database      Applicable
Event         Applicable
File          Not Applicable
Mail          Not Applicable
Notification  Not Applicable
```

Nếu sau này thêm email xác nhận đơn, Mail chuyển thành Applicable và app yêu cầu Mail ID, design spec, task, verification tương ứng.

## 9. Task context

Task phải nhận cả common baseline và business context cần thiết. Ví dụ backend task có context logic:

```text
Common:
  APP-ARCH-001
  BE-ARCH-001
  API-STD-001
  TX-STD-001
  AUTH-DES-001

Business:
  REQ/BR/AC
  NFR/DREQ/SREQ
  DES-INV-001
  API-DES-002
  DBD-001/002

Output:
  API-INV-001

Verification:
  TEST-INV-001
  PERF-001
  REL-TEST-001
```

App có thể tự resolve common docs theo scope thay vì ghi lặp vào từng task.

## 10. Tổng thể

Mô hình tài liệu nên được hiểu theo hai chiều song song:

```text
Dimension 1 — lifecycle
Business → Requirement → Design → Deliverable → Task → Verification → Operations

Dimension 2 — scope
Common/Application Design
        +
Business/Feature Design
```

Và các list `Screen/Batch/Interface/File/Mail/Notification/...` là các **management views theo loại output**, giúp project manager/architect kiểm tra completeness mà không phải duyệt từng folder.