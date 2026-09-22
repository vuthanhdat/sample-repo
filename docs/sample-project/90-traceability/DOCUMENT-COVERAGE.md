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
| Performance/scalability NFR | Baseline | `NFR-PERF-001` |
| Reliability/availability NFR | Baseline | `NFR-REL-001` |
| Observability/operability NFR | Baseline | `NFR-OPS-001` |
| Data requirements | Baseline | `DREQ-001` |
| Security requirements | Baseline | `SREQ-001` |
| Integration requirements | Baseline | `IREQ-001` |

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
| File/export design | Not Applicable | Recorded | Sample scope has no business file output |
| Reporting design | Not Applicable | Recorded | Sample scope has no business report deliverable |

## 3. Deliverable coverage

| Type | Deliverables | Spec coverage | Task coverage | Verification coverage |
|---|---|---|---|---|
| API | `API-ORD-001`, `API-INV-001` | Yes | Yes | Yes |
| Screen | `SCR-ORD-001` | Yes | Yes | Functional/Security/Performance |
| Event | `EVT-ORD-001` | Yes | Yes | Functional/Reliability |
| Database | `DB-ORD-001`, `DB-INV-001` | Yes | `TASK-DATA-001` | Migration/integrity/reliability via strategy |
| Job | `JOB-INV-001` | Yes | `TASK-JOB-001` | `REL-TEST-001` + operational checks |
| External Interface | `INT-SHP-001` | Yes | `TASK-INT-001` | `TEST-SHP-001` |

## 4. Planning / verification / operations coverage

| Product | Status |
|---|---|
| Roadmap/milestones | `ROADMAP-001` baseline |
| Implementation task set | Present |
| Test strategy | `TEST-STRAT-001` baseline |
| Functional verification | Present |
| Performance verification | `PERF-001` |
| Reliability verification | `REL-TEST-001` |
| Security verification | `SEC-TEST-001` |
| Integration contract verification | `TEST-SHP-001` |
| Deployment design | `DEP-001` |
| Observability design | `OBS-001` |
| Runbook | `RUN-001` |
| Change request example | `CR-001` |
| Traceability view | `TRACEABILITY.md` |

## 5. Why this view matters

App không nên chỉ hiển thị “có bao nhiêu Markdown file”. Nó phải biết ProjectTemplate yêu cầu những **document/design products** nào, product nào Applicable, product nào thiếu, product nào chưa Baseline và deliverable nào chưa có đủ specification/task/verification. Đây mới là document completeness có thể kiểm soát bằng machine.
