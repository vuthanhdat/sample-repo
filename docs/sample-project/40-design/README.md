# Design Structure

`40-design/` được tổ chức theo **scope trước, loại artifact sau**. Đây là ranh giới bắt buộc của project template.

```text
40-design/
├── README.md
├── 10-common/
│   ├── README.md
│   ├── architecture/
│   ├── backend/
│   ├── frontend/
│   ├── api/
│   └── security/
└── 20-business/
    ├── README.md
    ├── 00-shared-domain/
    │   ├── README.md
    │   ├── architecture/
    │   ├── data/
    │   └── configuration/
    ├── order/
    │   ├── README.md
    │   ├── decision/
    │   ├── api/
    │   ├── screen/
    │   └── event/
    ├── inventory/
    │   ├── README.md
    │   ├── decision/
    │   ├── api/
    │   └── job/
    └── shipping/
        ├── README.md
        └── integration/
```

## 1. `10-common` — Application-wide design

Chỉ chứa design/standard áp dụng cho toàn application hoặc một technical scope rộng, không mô tả riêng nghiệp vụ Order, Inventory hay Shipping.

Ví dụ: application architecture, backend/frontend architecture, API conventions, pagination, error contract, transaction/concurrency, login/logout/session, authorization/audit enforcement.

Common design là baseline được kế thừa. Feature không copy lại các rule này; feature chỉ khai báo exception khi thật sự cần.

```text
Common Design
    ↓ applies-to
Business Design
    ↓ specifies
Deliverable
    ↓ implemented-by
Task
```

## 2. `20-business` — Domain/feature design

Chứa mọi thiết kế xuất phát từ business requirement hoặc business domain. Nhánh này được tổ chức **domain-first** để tránh trộn tất cả API/screen/job của mọi feature vào một folder kỹ thuật duy nhất.

- `00-shared-domain`: business design dùng chung nhiều bounded context, ví dụ system/domain ownership, data model, physical schema, business configuration.
- `order`: solution decision, Order API, Order screen, Order event.
- `inventory`: stock reservation decision, Inventory API, reservation expiry job.
- `shipping`: external shipping integration.

## 3. Classification rule

Một tài liệu là **Common** khi nội dung vẫn hợp lệ nếu thay business domain bằng domain khác. Một tài liệu là **Business** khi nội dung chứa business concept, business state, business data ownership, business rule, business API/screen/job/event/interface cụ thể.

Ví dụ:

- Pagination response format → Common.
- Transaction/retry convention → Common.
- Login/logout/session → Common.
- `Order.PendingApproval` transition → Business/Order.
- `StockReservation` schema → Business/Shared Domain hoặc Inventory.
- Reservation expiry job → Business/Inventory.
- Shipping provider mapping → Business/Shipping.

## 4. Common inheritance

Task không cần liệt kê thủ công mọi common document. SaaS có thể resolve common baseline theo `appliesTo`:

```text
Project
  ├── Common Architecture
  ├── Backend Standards
  ├── Frontend Standards
  ├── API Standards
  ├── Transaction Standards
  └── Security/Auth Standards

Task
  ├── inherited common baseline
  └── explicit business read set
```

Nếu Common Design thay đổi, impact analysis phải tìm tất cả business design/deliverable/task nằm trong scope áp dụng.

## 5. Không phân loại theo file type ở root

Không được quay lại cấu trúc kiểu:

```text
40-design/api/
40-design/data/
40-design/screen/
40-design/job/
```

vì cấu trúc đó làm mất ranh giới Common vs Business và gom các bounded context khác nhau theo technical artifact. Artifact type chỉ là cấp con bên trong scope/domain thích hợp.