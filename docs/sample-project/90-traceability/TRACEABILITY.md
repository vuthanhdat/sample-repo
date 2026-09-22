# Traceability View

Đây là derived view minh họa graph tổng quát. Folder không quyết định dependency.

## Goal → Flow → Feature

| Goal | Flow | Feature |
|---|---|---|
| `GOAL-001` | `BF-001` | `FEATURE-001` |
| `GOAL-002` | `BF-002` | `FEATURE-002` |

## Feature → Requirement → Design → Deliverable → Task → Verification

| Feature | Requirements | Design | Deliverables | Tasks | Verification |
|---|---|---|---|---|---|
| `FEATURE-001` | `REQ-F001-001/002` | `DES-F001-001`, detailed specs | `SCR/API/EVT/DB-F001-*` | `TASK-F001-*` | `TEST-F001-001` + NFR tests |
| `FEATURE-002` | `REQ-F002-001`, `IREQ-001` | `DES-F002-001`, detailed specs | `API/JOB/INT/DB-F002-*` | `TASK-F002-*` | `TEST-F002-001` + NFR tests |

## Full Example Path

```text
GOAL-001
  ↓ decomposes-to
BF-001
  ↓ decomposes-to
FEATURE-001
  ↓ decomposes-to
REQ-F001-001
  ↓ satisfied-by
DES-F001-001
  ↓ introduces
API-F001-001
  ↑ specified-by
API-DES-F001-001
  ↓ implemented-by
TASK-F001-001
  ↓ verified-by
TEST-F001-001
```

Common Design such as `API-STD-001` or `TX-STD-001` adds cross-cutting edges to multiple feature designs/tasks.