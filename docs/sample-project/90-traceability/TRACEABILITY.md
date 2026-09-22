# Sample Project Traceability View

File này là **derived view** để con người kiểm tra nhanh traceability. Trong hệ thống thật, matrix này phải được generate từ relation graph, object registry và ProjectTemplate; không nhập tay như canonical source.

## 1. Goal → Flow → Functional Requirement

| Goal | Business Flow | Functional requirements |
|---|---|---|
| `GOAL-001` | `BF-001`, `BF-003` | `REQ-ORD-001`, `REQ-ORD-002`, `REQ-INV-001`, `REQ-ORD-003` qua các flow |
| `GOAL-002` | `BF-001`, `BF-002` | `REQ-INV-001` và Order requirements liên quan stock |
| `GOAL-003` | `BF-001`, `BF-003` | `REQ-ORD-003` |

## 2. Cross-cutting requirement constraints

Functional Requirement không phải input duy nhất của design. Các cross-cutting requirements constrain nhiều deliverable cùng lúc.

| Requirement | Constrains / satisfied by |
|---|---|
| `NFR-PERF-001` | Order/Inventory APIs, screen, DB design, job, `PERF-001` |
| `NFR-REL-001` | APIs, DBs, job, event, integration, `REL-TEST-001` |
| `NFR-OPS-001` | APIs, job, integration, `OBS-001`, `RUN-001` |
| `NFR-UX-001` | `SCR-DES-001`, `SCR-ORD-001`, `UX-TEST-001` |
| `NFR-MNT-001` | `ARCH-002`, implementation boundaries, `ARCH-TEST-001` |
| `DREQ-001` | `DBD-001`, `DBD-002`, `DB-ORD-001`, `DB-INV-001` |
| `SREQ-001` | `SEC-DES-001`, APIs, screen, `SEC-TEST-001` |
| `IREQ-001` | `INT-DES-001`, `INT-SHP-001`, `TEST-SHP-001` |

## 3. Requirement → Decision → Design Specification → Deliverable

| Requirement family | Design decision / architecture | Detailed specifications | Deliverables |
|---|---|---|---|
| Order functional | `DES-ORD-001`, `ARCH-001`, `ARCH-002` | `API-DES-001`, `SCR-DES-001`, `EVT-DES-001`, `DBD-001/002` | `API-ORD-001`, `SCR-ORD-001`, `EVT-ORD-001`, `DB-ORD-001` |
| Inventory functional | `DES-INV-001`, `ARCH-001`, `ARCH-002` | `API-DES-002`, `DBD-001/002`, `JOB-DES-001` | `API-INV-001`, `DB-INV-001`, `JOB-INV-001` |
| Shipping integration | `ARCH-001` | `INT-DES-001` | `INT-SHP-001` |
| Security | `ARCH-002` | `SEC-DES-001` | constrains APIs/screen/integration implementation |
| Business configuration | `DES-ORD-001`, `DES-INV-001` | `CFG-DES-001` | runtime governed configuration |

The important distinction is:

```text
Design Decision = why/which solution shape
Design Specification = exact contract/design of one aspect
Deliverable = output the system must actually contain
```

## 4. Deliverable → Task → Verification

| Deliverable | Implementing task | Primary verification |
|---|---|---|
| `DB-ORD-001` | `TASK-DATA-001` | migration/integrity + strategy/architecture gates |
| `DB-INV-001` | `TASK-DATA-001` | integration/concurrency + `REL-TEST-001` + architecture gates |
| `API-ORD-001` | `TASK-ORD-BE-001` | `TEST-ORD-001`, `PERF-001`, `REL-TEST-001`, `SEC-TEST-001`, `ARCH-TEST-001` |
| `EVT-ORD-001` | `TASK-ORD-BE-001` | `TEST-ORD-001`, `REL-TEST-001` |
| `SCR-ORD-001` | `TASK-ORD-FE-001` | Functional + `SEC-TEST-001`, `PERF-001`, `UX-TEST-001`, `ARCH-TEST-001` |
| `API-INV-001` | `TASK-INV-BE-001` | `TEST-INV-001`, `PERF-001`, `REL-TEST-001`, `SEC-TEST-001`, `ARCH-TEST-001` |
| `JOB-INV-001` | `TASK-JOB-001` | `REL-TEST-001` + operational readiness |
| `INT-SHP-001` | `TASK-INT-001` | `TEST-SHP-001` + reliability/operability evidence |

## 5. Planning / operations

```text
Design/Product Baselines
        ↓
ROADMAP-001
   ├── MS-001 Data Foundation
   ├── MS-002 Order Processing Flow
   └── MS-003 Integration & Operational Readiness
        ↓
Tasks
        ↓
Verification Runs
        ↓
DEP-001 + OBS-001 + RUN-001
        ↓
Release readiness
```

`TASK-OPS-001` implements production-readiness work around deployment, observability, alerts and runbook evidence; this work is not hidden inside backend feature tasks.

## 6. Example complete functional path

```text
GOAL-001
  ↓ contributes through
BF-001
  ↓ decomposes-to
REQ-ORD-002
  ├── governed-by → BR-ORD-001
  ├── accepted-by → AC-ORD-002-01/02
  ↓ satisfied-by
DES-ORD-001
  ↓ introduces
API-ORD-001
  ↑ specified-by
API-DES-001
  ↓ implemented-by
TASK-ORD-BE-001
  ↓ verified-by
TEST-ORD-001 / SEC-TEST-001 / REL-TEST-001 / ARCH-TEST-001
```

## 7. Example NFR path

```text
NFR-REL-001
  ├─ constrains → API-INV-001
  ├─ constrains → JOB-INV-001
  ├─ constrains → EVT-ORD-001
  └─ constrains → INT-SHP-001
         ↓
Detailed designs define idempotency/retry/concurrency
         ↓
Implementation tasks
         ↓
REL-TEST-001
```

Một requirement như `NFR-UX-001` hoặc `NFR-MNT-001` cũng đi theo graph tương tự tới Screen/Architecture/Task/Verification. NFR vì thế không nằm “bên lề” functional requirement; nó là một dimension của graph và có thể tác động tới nhiều output.

## 8. Change impact example

`CR-001` thay approval threshold. Candidate graph:

```text
CR-001
  ↓ changes
BR-ORD-001 / CFG-DES-001
  ↓ impacts
REQ-ORD-002
DES-ORD-001
API-DES-001 / API-ORD-001
SCR-DES-001 / SCR-ORD-001
TEST-ORD-001
```

Relation chỉ xác định **candidate impact**. Mỗi item phải có disposition như `Update Required`, `Review Required`, `Revalidation Required`, `Replan Required` hoặc `No Change Required`. Change closure giữ evidence của object versions và verification runs thực tế.

## 9. Coverage view

Xem `DOCUMENT-COVERAGE.md` để thấy ProjectTemplate/checklist coverage theo loại requirement, design product, deliverable, verification và operations document.
