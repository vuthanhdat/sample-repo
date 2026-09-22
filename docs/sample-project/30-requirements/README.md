# Requirements

Requirement được tổ chức theo **loại quyết định/constraint**, không theo domain mẫu.

```text
30-requirements/
├── functional/
│   ├── feature-001/
│   └── feature-002/
├── business-rules/
├── data/
├── integration/
├── non-functional/
└── security/
```

- `functional/`: behavior/capability mà feature phải cung cấp.
- `business-rules/`: policy/invariant có thể govern nhiều requirement hoặc feature.
- `data/`: data ownership, retention, quality, history, integrity expectations.
- `integration/`: interaction/exchange requirement với system/module khác.
- `non-functional/`: quality attributes như performance, reliability, usability, maintainability, compatibility, portability...
- `security/`: security/privacy-specific requirements.

Functional Requirement mô tả WHAT của feature. Business Rule và NFR/Data/Integration/Security có thể constrain nhiều feature và được liên kết bằng stable ID.

Rule local chỉ dùng trong một requirement có thể nằm trực tiếp trong requirement đó; chỉ tách thành `BR-*` khi rule có lifecycle/reuse/impact độc lập.