# Feature Design

Feature Design chứa HOW cho từng `FEATURE-*`. Cấu trúc vật lý là:

```text
20-feature/
├── feature-001/
└── feature-002/
```

Mỗi feature có thể có decision, API, screen, data, job, event, integration... tùy applicability. Feature Design kế thừa Common Design ở `../10-common/` và không lặp lại pagination/transaction/auth/error/logging standards chung.