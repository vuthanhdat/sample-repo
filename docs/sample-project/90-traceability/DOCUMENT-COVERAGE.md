# Document Coverage — Derived Checklist View

File này minh họa view mà SaaS nên **generate từ ProjectTemplate + document/object/deliverable registry**. Nó không phải source of truth riêng.

## 1. Business & requirement coverage

| Product | Status | Object/File |
|---|---|---|
| Project scope/context | Baseline | `README.md`, `BUSINESS-CONTEXT.md` |
| Business goals | Baseline | `GOAL-001..003` |
| Business capabilities | Baseline | `CAP-ORD-001`, `CAP-INV-001` |
| Business flows | Baseline | `BF-001..003` |
| Functional requirements | Baseline | `REQ-ORD-*`, `REQ-INV-*` |
| Business rules / AC | Baseline | `BR-*`, `AC-*` semantic objects |
| NFR applicability review | Baseline | `NFR-CHECKLIST.md` |
| Performance/scalability | Baseline | `NFR-PERF-001` |
| Reliability/availability/DR | Baseline | `NFR-REL-001` |
| Observability/operability | Baseline | `NFR-OPS-001` |
| Usability/accessibility | Baseline | `NFR-UX-001` |
| Maintainability/modifiability | Baseline | `NFR-MNT-001` |
| Data requirements | Baseline | `DREQ-001` |
| Security/privacy/audit requirements | Baseline | `SREQ-001` |
| Integration requirements | Baseline | `IREQ-001` |

## 2. Common / Application Design coverage

| Common design product | Applicability | Status | Object |
|---|---|---|---|
| Application architecture | Applicable | Baseline | `APP-ARCH-001` |
| Backend architecture | Applicable | Baseline | `BE-ARCH-001` |
| Frontend architecture | Applicable | Baseline | `FE-ARCH-001` |
| API conventions / pagination / error | Applicable | Baseline | `API-STD-001` |
| Transaction / concurrency | Applicable | Baseline | `TX-STD-001` |
| Authentication / login / logout / session | Applicable | Baseline | `AUTH-DES-001` |

Common Design là baseline kỹ thuật được Business Design và Task kế thừa theo scope. Common document thay đổi phải chạy impact analysis tới các object trong `appliesTo`.

## 3. Business / Feature Design coverage

| Design product | Applicability | Status | Specification |
|---|---|---|---|
| System/domain context | Applicable | Baseline | `ARCH-001` |
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
| Business authorization/audit | Applicable | Baseline | `SEC-DES-001` |
| Business configuration | Applicable | Baseline | `CFG-DES-001` |
| Deployment design | Applicable | Baseline | `DEP-001` |
| Observability design | Applicable | Baseline | `OBS-001` |
| File/export design | Not Applicable | Recorded | no business file output |
| Reporting design | Not Applicable | Recorded | no report deliverable |
| Mail design | Not Applicable | Recorded | no email deliverable |
| Notification design | Not Applicable | Recorded | no notification deliverable |

## 4. Output inventory coverage

| Inventory | Applicability | Entries / Result | Coverage |
|---|---|---|---|
| Screen List | Applicable | `SCR-ORD-001` | Design + Task present |
| API List | Applicable | `API-ORD-001`, `API-INV-001` | Design + Task + Verification present |
| Batch/Job List | Applicable | `JOB-INV-001` | Design + Task + Reliability verification present |
| Interface List | Applicable | `INT-SHP-001` | Design + Task + Contract verification present |
| Event List | Applicable | `EVT-ORD-001` | Design + Task present |
| Database List | Applicable | `DB-ORD-001`, `DB-INV-001` | DBD + Migration task present |
| File List | Not Applicable | None | Reason recorded |
| Mail List | Not Applicable | None | Reason recorded |
| Notification List | Not Applicable | None | Reason recorded |

Inventory documents nằm ở `50-deliverables/00-lists/`. Trong SaaS thật chúng nên được generate từ Deliverable registry, không nhập tay làm canonical source.

## 5. Deliverable coverage

| Type | Deliverables | Specification | Task | Verification |
|---|---|---|---|---|
| API | `API-ORD-001`, `API-INV-001` | `API-DES-001/002` | Backend tasks | Functional + Performance + Reliability + Security + Architecture |
| Screen | `SCR-ORD-001` | `SCR-DES-001` | `TASK-ORD-FE-001` | Functional + Security + UX |
| Event | `EVT-ORD-001` | `EVT-DES-001` | `TASK-ORD-BE-001` | Functional + Reliability |
| Database | `DB-ORD-001`, `DB-INV-001` | `DBD-001/002` | `TASK-DATA-001` | Integrity + Reliability/Concurrency |
| Job | `JOB-INV-001` | `JOB-DES-001` | `TASK-JOB-001` | Reliability + Operational readiness |
| External Interface | `INT-SHP-001` | `INT-DES-001` | `TASK-INT-001` | Contract + Reliability/Operability |

## 6. Planning / verification / operations coverage

| Product | Status / source |
|---|---|
| Roadmap/milestones | `ROADMAP-001` baseline |
| Implementation task set | Present |
| Test strategy | `TEST-STRAT-001` baseline |
| Functional verification | `TEST-ORD-001`, `TEST-INV-001`, E2E scenarios |
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

## 7. Coverage interpretation

Coverage không có nghĩa `file tồn tại = done`. App phải biết:

```text
Requirement coverage
+ Common Design baseline coverage
+ Business Design coverage
+ Output Inventory coverage
+ Task coverage
+ Verification coverage
+ Operations readiness
```

Một deliverable thiếu design/task/test phải được flag. Một Common Design item chưa evaluate cũng phải được flag vì blast radius của nó có thể là toàn application.