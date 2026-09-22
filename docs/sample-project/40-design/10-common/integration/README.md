# Integration Design Area

Integration Design chứa các quyết định chung cho giao tiếp giữa system/module/service, tách khỏi feature-specific integration specification.

Recommended decision areas:

```text
integration/
├── synchronous/       # HTTP/RPC/request-response conventions
├── asynchronous/      # event/message conventions and delivery semantics
├── file-batch/        # file transfer/import/export/batch exchange conventions
└── resilience/        # timeout, retry, circuit breaking, idempotency, duplicate handling
```

Các quyết định thường gồm protocol selection, contract/versioning, ownership, error mapping, delivery guarantee, ordering, idempotency, timeout/retry và compatibility policy.

Feature-specific endpoint/event/interface contract vẫn thuộc `20-feature/<feature>/...`.