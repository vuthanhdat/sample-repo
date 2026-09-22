# Order Design

Owner scope: Order Management.

```text
order/
├── README.md
├── decision/
│   └── DES-ORD-001-order-approval-solution.md
├── api/
│   └── API-DES-001-order-command-api.md
├── screen/
│   └── SCR-DES-001-order-detail.md
└── event/
    └── EVT-DES-001-order-status-changed.md
```

Các tài liệu ở đây chỉ mô tả solution đặc thù của Order: state transition, approval, Order API, Order UI và Order event. Pagination/error/auth/transaction conventions được kế thừa từ `10-common`.

Nếu sau này Order có mail, notification, report hoặc batch riêng thì detailed design tương ứng cũng nằm dưới `order/<artifact-type>/`, còn inventory tổng hợp vẫn được generate ở `50-deliverables/00-lists/`.