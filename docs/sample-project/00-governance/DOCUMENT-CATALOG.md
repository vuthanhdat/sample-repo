# Document Catalog — Order & Inventory Management

Tài liệu này là **checklist/canonical catalog** cho bộ tài liệu của sample project. Mục tiêu không phải bắt mọi project phải có cùng số file, mà định nghĩa **những loại knowledge/design/work product nào phải được xem xét**, điều kiện nào khiến chúng trở thành bắt buộc và chúng liên kết với nhau ra sao.

## 1. Nguyên tắc

Một project software đầy đủ không chỉ có Functional Requirement và một vài design note. Tối thiểu phải xem xét các chiều sau:

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
Architecture & Design
  ├── System / Component
  ├── Data / Database
  ├── API
  ├── Screen / UX
  ├── Batch / Job
  ├── Event / Messaging
  ├── External Interface
  ├── Security
  ├── Configuration
  └── Deployment / Operations
  ↓
Deliverable Inventory
  ↓
Roadmap / Milestone / Task
  ↓
Verification
  ├── Functional
  ├── Integration / Contract
  ├── E2E
  ├── Performance
  ├── Security
  └── Operational readiness
  ↓
Release / Operations / Change / Traceability
```

## 2. Document checklist

| Layer | Document / Object type | ID prefix | Khi nào bắt buộc | Sample |
|---|---|---|---|---|
| Governance | Project charter / scope | `PRJ-` | Mọi project | `README.md` |
| Governance | Document catalog | `DOC-CAT-` | Mọi project template | File này |
| Business | Business goal | `GOAL-` | Mọi project có business outcome | `10-goals/*` |
| Business | Actor / glossary / context | `ACT-`, `TERM-` | Khi domain có actor/thuật ngữ nghiệp vụ | `15-business-context/BUSINESS-CONTEXT.md` |
| Business | Business flow | `BF-` | Khi behavior đi qua nhiều bước/actor/system | `20-business-flows/*` |
| Requirement | Functional requirement | `REQ-` | Khi hệ thống phải cung cấp behavior | `30-requirements/REQ-*` |
| Requirement | Business rule | `BR-` | Khi behavior bị chi phối bởi rule độc lập | Embedded/reference từ requirement |
| Requirement | Acceptance criterion | `AC-` | Mọi requirement có thể kiểm chứng | Embedded/reference từ requirement |
| Requirement | Non-functional requirement | `NFR-` | Mọi production system | `30-requirements/non-functional/*` |
| Requirement | Data requirement | `DREQ-` | Khi có persistence, history, retention, ownership | `30-requirements/data/*` |
| Requirement | Integration requirement | `IREQ-` | Khi giao tiếp external system | `30-requirements/integration/*` |
| Requirement | Security/compliance requirement | `SREQ-` | Khi có auth, PII, audit, compliance | `30-requirements/security/*` |
| Architecture | System context | `ARCH-` | Mọi system không-trivial | `40-design/architecture/*` |
| Architecture | Component/module architecture | `ARCH-` | Khi có nhiều module/component | `40-design/architecture/*` |
| Design | Design decision | `DES-` | Khi requirement cần quyết định solution | Existing `DES-*` |
| Design | Logical data model | `DBD-` | Khi có data domain/persistence | `40-design/data/*` |
| Design | Physical DB schema | `DBD-` | Khi implement relational DB | `40-design/data/*` |
| Design | API specification | `API-DES-` | Khi có API deliverable | `40-design/api/*` |
| Design | Screen specification | `SCR-DES-` | Khi có UI screen | `40-design/screen/*` |
| Design | Job specification | `JOB-DES-` | Khi có scheduled/background processing | `40-design/job/*` |
| Design | Event specification | `EVT-DES-` | Khi publish/consume message/event | `40-design/event/*` |
| Design | Interface specification | `INT-DES-` | Khi tích hợp external system/file/protocol | `40-design/integration/*` |
| Design | Security design | `SEC-DES-` | Khi có authorization/audit/security boundary | `40-design/security/*` |
| Design | Configuration design | `CFG-DES-` | Khi behavior phải configurable | `40-design/configuration/*` |
| Deliverable | Screen/API/Event/Job/DB/etc. | `SCR-/API-/EVT-/JOB-/DB-/INT-` | Derived từ design | `50-deliverables/*` |
| Planning | Roadmap / milestone | `ROADMAP-`, `MS-` | Project triển khai theo phase/release | `55-planning/*` |
| Execution | Task | `TASK-` | Khi có work phải thực thi | `60-tasks/*` |
| Verification | Test strategy | `TEST-STRAT-` | Mọi project production | `70-verification/TEST-STRATEGY.md` |
| Verification | Verification definition | `TEST-`, `PERF-`, `SEC-TEST-` | Theo requirement/deliverable | `70-verification/*` |
| Operations | Deployment design | `DEP-` | Có deployable runtime | `80-operations/*` |
| Operations | Observability / runbook | `OBS-`, `RUN-` | Production system | `80-operations/*` |
| Change | Change request | `CR-` | Baseline object cần thay đổi | `85-change-management/*` |
| Traceability | Coverage / impact view | derived | Mọi governed project | `90-traceability/*` |

## 3. Applicability rule

Catalog là checklist **có điều kiện**, không phải bureaucracy bắt buộc. Ví dụ project không có background processing thì `JOB-DES-*` và `JOB-*` có trạng thái `Not Applicable`, không cần tạo file giả. App nên quản lý trạng thái của từng document product:

```text
Not Evaluated → Applicable → Draft → Baseline
              ↘ Not Applicable (reason required)
```

## 4. Coverage rule

Một project chỉ được coi là design-ready khi:

1. Mọi Functional Requirement có Acceptance Criteria.
2. NFR categories đã được review và mỗi category là `Applicable` hoặc `Not Applicable` có lý do.
3. Data ownership, retention và audit requirement đã rõ nếu có persistence.
4. Mọi external integration có contract/error/retry/idempotency requirement.
5. Mọi planned deliverable có design product phù hợp với type của nó.
6. Database, job, event, screen, API không được implementation tự phát minh mà không có design/trace source.
7. Verification strategy cover cả functional lẫn NFR quan trọng.
8. Deployment/operations readiness được xem xét trước production release.

## 5. Traceability không phải cây folder

Folder chỉ giúp con người duyệt tài liệu. Quan hệ thật là graph. Ví dụ `NFR-PERF-001` có thể đồng thời constrain `API-ORD-001`, `API-INV-001`, `DB-ORD-001` và `JOB-INV-001`; do đó nó không nằm "dưới" riêng một API hay DB nào.

Project Template của SaaS sau này nên dùng catalog này để sinh checklist và coverage report tự động.