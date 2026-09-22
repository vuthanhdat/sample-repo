# Requirements

Requirement được tổ chức theo **scope**, không theo domain mẫu.

```text
30-requirements/
├── functional/
│   ├── feature-001/
│   └── feature-002/
├── non-functional/
├── data/
├── integration/
└── security/
```

Functional Requirement mô tả behavior của feature. NFR/Data/Integration/Security có thể constrain nhiều feature và được liên kết bằng ID.