# Design Structure — Common vs Business

`40-design` được chia theo **scope của quyết định thiết kế**, không chỉ theo loại artifact.

## 1. Common / Application Design

`00-common/` chứa các rule và architecture áp dụng mặc định cho toàn application hoặc nhiều module. Đây là baseline kỹ thuật mà mọi feature/task phải tuân thủ trừ khi có một approved exception/ADR.

Ví dụ:

- Application architecture và dependency direction.
- Backend architecture và responsibility của Controller/Application/Domain/Infrastructure.
- Frontend architecture, routing, state/data fetching, form, i18n và UI composition.
- API conventions: naming, pagination, sorting/filtering, error model, idempotency.
- Transaction/concurrency rules.
- Authentication/session/login/logout.
- Cross-cutting concerns: logging, audit, validation, authorization, configuration.

Common Design **không mô tả nghiệp vụ Order/Inventory cụ thể**.

## 2. Business / Feature Design

Các design product nghiệp vụ mô tả solution cho requirement/deliverable cụ thể: Order approval, stock reservation, Order API, Order Detail screen, expiry job, event contract, shipping interface, physical schema...

Trong sample hiện tại, các file `DES-*` và các folder `api/`, `data/`, `screen/`, `job/`, `event/`, `integration/`, `configuration/` là Business/Feature Design. Khi SaaS sinh project mới, template nên biểu diễn chúng dưới logical scope `BusinessDesign`; physical export có thể tiếp tục nhóm theo artifact type để dễ duyệt.

## 3. Inheritance rule

Business design không lặp lại common rules. Ví dụ `API-DES-001` chỉ mô tả endpoint/business contract của Order; pagination/error/idempotency chung được kế thừa từ `API-STD-001`. Một design chỉ override common baseline khi ghi rõ exception và relation tới decision/ADR phù hợp.

```text
Common Design Baseline
        ↓ applies-to
Business / Feature Design
        ↓ specifies
Deliverable
        ↓ implemented-by
Task
```

## 4. Scope classification

Mỗi design document trong app nên có field:

```text
scope = Common | Business | Integration | Operations
```

và có thể có:

```text
appliesTo = Project | Module | DeliverableType | Deliverable IDs
```

Nhờ đó một thay đổi common như transaction policy hoặc pagination convention có thể chạy impact analysis tới toàn bộ design/deliverable/task liên quan.