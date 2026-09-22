# Project Context Taxonomy

Context mô tả thế giới mà solution tồn tại trong đó. Không gom toàn bộ context vào một mega-document; tách theo loại knowledge vì scope, actor, external dependency và assumption thường thay đổi độc lập.

```text
15-context/
├── PROJECT-CONTEXT.md
├── SCOPE.md
├── ACTOR-LIST.md
├── EXTERNAL-SYSTEMS.md
├── ASSUMPTIONS-CONSTRAINTS.md
└── GLOSSARY.md
```

- `PROJECT-CONTEXT.md`: background, problem space, organization/environment tổng quan.
- `SCOPE.md`: system boundary, in-scope/out-of-scope.
- `ACTOR-LIST.md`: human/system actors và responsibilities ở mức context.
- `EXTERNAL-SYSTEMS.md`: systems/services bên ngoài boundary và dependency relationship.
- `ASSUMPTIONS-CONSTRAINTS.md`: assumptions và constraints đã biết.
- `GLOSSARY.md`: ubiquitous language/terminology.

Context nói **điều gì tồn tại và giới hạn nào đang có**, không mô tả solution design.