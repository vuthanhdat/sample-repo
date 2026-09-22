# Output Inventory Lists

Folder này chứa các **tài liệu danh mục tổng hợp** theo loại output. Chúng phục vụ quản lý coverage và navigation, không thay thế detailed design/specification.

## Lists

- `SCREEN-LIST.md` — màn hình/page.
- `API-LIST.md` — API/service endpoints/contracts.
- `BATCH-JOB-LIST.md` — batch, scheduler, background worker/job.
- `INTERFACE-LIST.md` — giao tiếp system-to-system.
- `EVENT-LIST.md` — event/message.
- `DATABASE-LIST.md` — database/schema/data store.
- `FILE-LIST.md` — file import/export.
- `MAIL-LIST.md` — email output.
- `NOTIFICATION-LIST.md` — in-app/push/system notification.

## Principle

Trong SaaS thật, các list nên là **generated views** từ Deliverable entities. User tạo/sửa Deliverable ở canonical registry; list được query/render theo type.

Mỗi row phải trace được tối thiểu:

```text
Requirement
   ↓
Design Specification
   ↓
Deliverable ID
   ↓
Task
   ↓
Verification
   ↓
Lifecycle Status
```

Nhờ vậy list không chỉ là bảng tên output mà còn là control sheet để phát hiện output thiếu design, task hoặc verification.