# Document Coverage — Derived Checklist View

File này minh họa view mà SaaS nên **generate từ ProjectTemplate + document/object registry**. Nó không phải source of truth riêng.

## 1. Business & requirement coverage

| Product | Status | Object/File |
|---|---|---|
| Project scope/context | Baseline | `README.md`, `BUSINESS-CONTEXT.md` |
| Business goals | Baseline | `GOAL-001..003` |
| Business capabilities | Baseline | `CAP-ORD-001`, `CAP-INV-001` |
| Business flows | Baseline | `BF-001..003` |
| Functional requirements | Baseline | `REQ-ORD-*`, `REQ-INV-*` |
| Business rules / AC | Baseline | `BR-*`, `AC-*` embedded semantic objects |
| NFR applicability review | Baseline | `NFR-CHECKLIST.md` |
| Performance/scalability | Baseline | `NFR-PERF-001` |
| Reliability/availability/DR | Baseline | `NFR-REL-001` |
| Observability/operability | Baseline | `NFR-OPS-001` |
| Usability/accessibility/browser baseline | Baseline | `NFR-UX-001` |
| Maintainability/modifiability | Baseline | `NFR-MNT-001` |
| Data requirements | Baseline | `DREQ-001` |
| Security/privacy/audit requirements | Baseline | `SREQ-001` |
| Integration requirements | Baseline | `IREQ-001` |
| Localization/i18n | Not Applicable | rationale in `NFR-CHECKLIST.md` |
| Regulated-industry compliance | Not Applicable | rationale in `NFR-CHECKLIST.md` |
| Offline operation | Not Applicable | rationale in `NFR-CHECKLIST.md` |

## 2. Architecture/design coverage

| Design product | Applicability | Status | Specification |
|---|---|---|---|
| System context | Applicable | Baseline | `ARCH-001` |
| Component boundaries | Applicable | Baseline | `ARCH-002` |
| Order solution decision | Applicable | Baseline | `DES-ORD-001` |
| Inventory solution decision | Applicable | Baseline | `DES-INV-001` |
| Logical data model | Applicable | Baseline | `DBD-001` |
| Physical DB design | Applicable | Baseline | `DBD-002` |
| Order API design | Applicable | Baseline | `API-DES-001` |
| Inventory API design | Applicable | Baseline | `API-DES-002` |
| Screen design | Applicable | Baseline | `SCR-DES-001` |
| Background job design | Applicable | Baseline | `JOB-DES-001` |
| Event contract | Applicable | Baseline | `EVT-DES-001` |
| External interface | Applicable | Baseline | `INT-DES-001` |
| Security design | Applicable | Baseline | `SEC-DES-001` |
| Configuration design | Applicable | Baseline | `CFG-DES-001` |
| Deployment design | Applicable | Baseline | `DEP-001` |
| Observability design | Applicable | Baseline | `OBS-001` |
| File/export design | Not Applicable | Recorded | no business file output in current sample |
| Reporting design | Not Applicable | Recorded | no business report deliverable in current sample |

## 3. Deliverable coverage

| Type | Deliverables | Specification | Task | Verification |
|---|---|---|---|---|
| API | `API-ORD-001`, `API-INV-001` | `API-DES-001/002` | Backend tasks | Functional + Performance + Reliability + Security + Architecture |
| Screen | `SCR-ORD-001` | `SCR-DES-001` | `TASK-ORD-FE-001` | Functional + Security + Performance + `UX-TEST-001` + Architecture |
| Event | `EVT-ORD-001` | `EVT-DES-001` | `TASK-ORD-BE-001` | Functional + `REL-TEST-001` |
| Database | `DB-ORD-001`, `DB-INV-001` | `DBD-001/002` | `TASK-DATA-001` | Migration/integrity + reliability/concurrency + architecture/static gates |
| Job | `JOB-INV-001` | `JOB-DES-001` | `TASK-JOB-001` | `REL-TEST-001` + operational readiness |
| External Interface | `INT-SHP-001` | `INT-DES-001` | `TASK-INT-001` | `TEST-SHP-001` + reliability/operability evidence |

## 4. Planning / verification / operations coverage

| Product | Status / source |
|---|---|
| Roadmap/milestones | `ROADMAP-001` baseline |
| Implementation task set | Present, including data/job/integration/operations tasks |
| Test strategy | `TEST-STRAT-001` baseline |
| Functional verification | `TEST-ORD-001`, `TEST-INV-001`, E2E task/scenarios |
| Integration contract verification | `TEST-SHP-001` |
| Performance verification | `PERF-001` |
| Reliability verification | `REL-TEST-001` |
| Security verification | `SEC-TEST-001` |
| Usability/accessibility verification | `UX-TEST-001` |
| Architecture/maintainability verification | `ARCH-TEST-001` |
| Deployment design | `DEP-001` |
| Observability design | `OBS-001` |
| Runbook | `RUN-001` |
| Production-readiness task | `TASK-OPS-001` |
| Change request example | `CR-001` |
| Traceability view | `TRACEABILITY.md` |

## 5. Coverage interpretation

Coverage không có nghĩa “file tồn tại = done”. Với từng document product, app cần biết:

```text
Applicability
→ lifecycle/status
→ required relations
→ baseline/version
→ downstream coverage
```

Ví dụ `JOB-DES-001` có file nhưng nếu không `specifies → JOB-INV-001`, hoặc `JOB-INV-001` không có implementation task/verification, coverage vẫn incomplete.

App không nên chỉ đếm Markdown. Nó phải biết ProjectTemplate yêu cầu những **knowledge/design/work products** nào, product nào Applicable, product nào thiếu, product nào chưa Baseline và deliverable nào chưa đủ specification/task/verification. Đây mới là document completeness có thể kiểm soát bằng machine.
