# Shared Business Domain Design

`00-shared-domain/` vẫn là **Business Design**, không phải Common Technical Design. Nó chứa các design mang business meaning nhưng được nhiều bounded context sử dụng chung.

## Contents

```text
00-shared-domain/
├── README.md
├── architecture/
│   ├── ARCH-001-system-context.md
│   └── ARCH-002-component-boundaries.md
├── data/
│   ├── DBD-001-logical-data-model.md
│   └── DBD-002-physical-schema.md
└── configuration/
    └── CFG-DES-001-business-configuration.md
```

## Why these are not Common

`ARCH-001/002` nói trực tiếp về ownership của Order, Inventory và Shipping. `DBD-*` chứa business entities/tables. `CFG-DES-001` chứa approval threshold, reservation TTL và shipping service level. Vì vậy chúng không thể tái sử dụng nguyên trạng cho một domain khác và không được đặt dưới `10-common`.

## Rule

Chỉ đặt artifact ở đây khi nó thực sự span nhiều business domains. Nếu artifact có một owner rõ như Order hoặc Inventory thì đặt vào folder owner tương ứng.