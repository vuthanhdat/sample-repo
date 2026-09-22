# Common / Application Design

`10-common/` là technical baseline của toàn application. Nội dung ở đây không được chứa flow nghiệp vụ riêng của Order, Inventory hay Shipping trừ ví dụ minh họa nhỏ.

## Structure

```text
10-common/
├── README.md
├── architecture/
│   └── APP-ARCH-001-application-architecture.md
├── backend/
│   ├── BE-ARCH-001-backend-design.md
│   └── TX-STD-001-transaction-concurrency.md
├── frontend/
│   └── FE-ARCH-001-frontend-design.md
├── api/
│   └── API-STD-001-api-conventions.md
└── security/
    ├── AUTH-DES-001-authentication-session.md
    └── SEC-DES-001-authorization-audit.md
```

## Rules

1. Common Design có stable ID, version và baseline như mọi design object khác.
2. Business Design mặc định kế thừa Common Design theo scope.
3. Common rule không được copy vào từng feature; chỉ reference bằng ID.
4. Exception phải explicit và trace được tới design decision/change request.
5. Thay đổi Common Design có blast radius lớn, do đó luôn cần impact analysis.

## Typical questions

- App được chia layer/module như thế nào?
- Backend responsibility đặt ở Controller/Application/Domain/Infrastructure ra sao?
- Frontend tổ chức shell/routing/state/form/data-fetching thế nào?
- Pagination/filter/sort/error response/idempotency dùng chuẩn nào?
- Transaction boundary, concurrency và retry policy là gì?
- Login/logout/session/token lifecycle hoạt động thế nào?
- Authorization và audit được enforce ở đâu?

Nếu câu hỏi chỉ có thể trả lời bằng khái niệm nghiệp vụ cụ thể, tài liệu đó thường không thuộc `10-common`.