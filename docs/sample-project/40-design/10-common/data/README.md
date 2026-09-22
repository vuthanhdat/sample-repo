# Data Design Area

Data design được tách theo các nhóm quyết định để tránh gom toàn bộ vào một data standard duy nhất.

```text
data/
├── modeling/
├── persistence/
├── lifecycle-history/
└── migration/
```

- `modeling`: conceptual/logical model, identity, relationships, ownership.
- `persistence`: physical storage/schema/indexing/access-pattern decisions.
- `lifecycle-history`: retention, archival, history, audit, soft delete/versioning.
- `migration`: schema/data evolution, compatibility và migration strategy.

`DATA-STD-001.md` giữ common baseline. Feature-specific entities/tables và access patterns vẫn thuộc `20-feature/<feature>/data/`.