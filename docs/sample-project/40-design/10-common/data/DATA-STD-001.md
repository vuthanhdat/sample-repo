---
id: DATA-STD-001
type: CommonStandard
scope: Data
status: Draft
version: 1
---
# DATA-STD-001 — Data & Database Standards

- Technical PK and business unique keys are distinct concepts.
- Ownership of each table/data object must be explicit.
- Schema naming, audit timestamps, soft delete, history and retention follow project conventions.
- DB default must not silently define business behavior that application code depends on unknowingly.
- Migration is versioned implementation artifact; destructive change requires migration/rollback strategy.
- Indexes are justified by access pattern, not created indiscriminately.