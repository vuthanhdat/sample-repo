# Inventory Design

Owner scope: Inventory Management.

```text
inventory/
├── README.md
├── decision/
│   └── DES-INV-001-stock-reservation-solution.md
├── api/
│   └── API-DES-002-stock-reservation-api.md
└── job/
    └── JOB-DES-001-expired-reservation-release.md
```

Các tài liệu ở đây mô tả stock reservation, Inventory API và reservation expiry processing. Transaction/concurrency/retry/job execution principles chung được kế thừa từ `10-common`; file trong Inventory chỉ ghi business-specific behavior, lock/invariant hoặc exception cần thiết.