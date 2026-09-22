# Document Catalog — Order & Inventory Management

Catalog này là checklist/canonical taxonomy cho ProjectTemplate. Mục tiêu là định nghĩa loại knowledge/design/work product nào cần được xem xét, applicability rule và quan hệ bắt buộc giữa chúng.

## 1. Lifecycle model

```text
Business Context
  ↓
Goals / Capabilities / Flows / Rules
  ↓
Requirements
  ├── Functional
  ├── Non-functional
  ├── Data
  ├── Integration
  └── Security / Compliance
  ↓
Design
  ├── Common / Application Design
  └── Business / Domain Design
  ↓
Deliverable Inventory
  ↓
Roadmap / Milestone / Task
  ↓
Verification
  ↓
Operations / Change / Traceability
```

## 2. Design taxonomy

### 2.1 Common / Application Design

Physical root: `40-design/10-common/`.

| Product | Prefix | Path | Applicability |
|---|---|---|---|
| Application Architecture | `APP-ARCH-` | `10-common/architecture/` | mọi app không-trivial |
| Backend Architecture | `BE-ARCH-` | `10-common/backend/` | có backend |
| Frontend Architecture | `FE-ARCH-` | `10-common/frontend/` | có frontend |
| API Conventions | `API-STD-` | `10-common/api/` | có API |
| Transaction/Concurrency Standard | `TX-STD-` | `10-common/backend/` | có write/persistence/job |
| Authentication/Session | `AUTH-DES-` | `10-common/security/` | có authenticated user |
| Authorization/Audit Enforcement | `SEC-DES-` | `10-common/security/` | có protected operations/audit |

Common Design chỉ chứa technical baseline có thể áp dụng qua nhiều business domains. Nó được business design/task kế thừa theo scope.

### 2.2 Business / Domain Design

Physical root: `40-design/20-business/`. Business branch luôn tổ chức `domain → artifact type`.

| Scope | Product | Prefix | Path |
|---|---|---|---|
| Shared Domain | System/domain architecture | `ARCH-` | `20-business/00-shared-domain/architecture/` |
| Shared Domain | Logical/physical DB design | `DBD-` | `20-business/00-shared-domain/data/` |
| Shared Domain | Business configuration | `CFG-DES-` | `20-business/00-shared-domain/configuration/` |
| Order | Solution decision | `DES-ORD-` | `20-business/order/decision/` |
| Order | API specification | `API-DES-` | `20-business/order/api/` |
| Order | Screen specification | `SCR-DES-` | `20-business/order/screen/` |
| Order | Event specification | `EVT-DES-` | `20-business/order/event/` |
| Inventory | Solution decision | `DES-INV-` | `20-business/inventory/decision/` |
| Inventory | API specification | `API-DES-` | `20-business/inventory/api/` |
| Inventory | Job specification | `JOB-DES-` | `20-business/inventory/job/` |
| Shipping | Interface specification | `INT-DES-` | `20-business/shipping/integration/` |

Nếu một design artifact chứa business entity/state/rule/ownership cụ thể thì không được đặt dưới `10-common`.

## 3. Business/requirement catalog

| Layer | Product | Prefix | Sample |
|---|---|---|---|
| Governance | Project scope | `PRJ-` | `README.md` |
| Business | Goal | `GOAL-` | `10-goals/*` |
| Business | Actor/term/context | `ACT-`, `TERM-` | `15-business-context/*` |
| Business | Business Flow | `BF-` | `20-business-flows/*` |
| Requirement | Functional Requirement | `REQ-` | `30-requirements/REQ-*` |
| Requirement | Business Rule | `BR-` | semantic object/reference |
| Requirement | Acceptance Criterion | `AC-` | semantic object/reference |
| Requirement | NFR | `NFR-` | `30-requirements/non-functional/*` |
| Requirement | Data Requirement | `DREQ-` | `30-requirements/data/*` |
| Requirement | Integration Requirement | `IREQ-` | `30-requirements/integration/*` |
| Requirement | Security Requirement | `SREQ-` | `30-requirements/security/*` |

## 4. Deliverable and inventory catalog

Detailed deliverables live in `50-deliverables/`. Type lists live in `50-deliverables/00-lists/` and should be generated from the Deliverable Registry in the SaaS.

| Inventory View | Output types |
|---|---|
| `SCREEN-LIST.md` | Screen/Page |
| `API-LIST.md` | API/Service contract |
| `BATCH-JOB-LIST.md` | Batch/Job/Scheduler |
| `INTERFACE-LIST.md` | External/System Interface |
| `EVENT-LIST.md` | Event/Message |
| `DATABASE-LIST.md` | Database/Schema/Data Store |
| `FILE-LIST.md` | Import/Export File |
| `MAIL-LIST.md` | Email |
| `NOTIFICATION-LIST.md` | In-app/Push/System Notification |

List là derived management view; detailed design vẫn nằm trong `40-design/20-business/<domain>/<artifact-type>/`.

## 5. Remaining catalog

| Layer | Product | Prefix / Location |
|---|---|---|
| Planning | Roadmap/Milestone | `ROADMAP-`, `MS-`, `55-planning/` |
| Execution | Task | `TASK-`, `60-tasks/` |
| Verification | Test Strategy / Verification | `TEST-STRAT-`, `TEST-`, `PERF-`, `SEC-TEST-`, `70-verification/` |
| Operations | Deployment/Observability/Runbook | `DEP-`, `OBS-`, `RUN-`, `80-operations/` |
| Change | Change Request | `CR-`, `85-change-management/` |
| Traceability | Coverage/Impact Views | derived, `90-traceability/` |

## 6. Applicability and coverage

```text
Not Evaluated
   ├── Applicable → Draft → Baseline
   └── Not Applicable + reason
```

Design-ready tối thiểu yêu cầu:

1. Functional requirements có acceptance criteria.
2. NFR categories đã evaluate.
3. Common Design baseline applicable đã Baseline hoặc explicitly waived.
4. Business Design được phân đúng domain owner.
5. Mọi planned deliverable có detailed design phù hợp type.
6. Deliverable có task + verification coverage.
7. Output inventory không có orphan item.
8. Operations readiness được đánh giá trước production.

## 7. Classification test

Khi phân loại một design document, hỏi theo thứ tự:

1. Nội dung này có còn đúng nếu thay Order/Inventory bằng một domain hoàn toàn khác không? Nếu có, candidate là Common.
2. Nội dung có business entity/state/rule/data ownership cụ thể không? Nếu có, là Business.
3. Nếu Business, owner domain là ai? Đặt dưới domain đó.
4. Nếu span nhiều business domains nhưng vẫn mang business meaning, đặt dưới `20-business/00-shared-domain`.

Folder là navigation; stable ID và relation graph vẫn là source of truth logic.