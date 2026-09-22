# Backend Design Area

Backend design có thể được breakdown theo các decision area sau khi project đủ lớn:

```text
backend/
├── architecture/              # backend layering/module/use-case conventions
├── persistence/               # repository/ORM/query/write persistence decisions
├── transaction-concurrency/   # transaction boundary, locking, optimistic concurrency
└── background-processing/     # worker, scheduler, background job processing conventions
```

Các file hiện có như `BE-ARCH-001.md` và `TX-STD-001.md` vẫn giữ stable ID. Project thật có thể đặt/move document vào decision area phù hợp vì path chỉ là navigation, không phải identity.

Không đưa API protocol, data model, integration protocol hay infrastructure topology vào backend document nếu chúng đã có taxonomy riêng.