# Document Catalog — Order & Inventory Management

Tài liệu này là **checklist/canonical catalog** cho bộ tài liệu của sample project. Mục tiêu không phải bắt mọi project phải có cùng số file, mà định nghĩa những loại knowledge/design/work product nào phải được xem xét, điều kiện nào khiến chúng trở thành bắt buộc và chúng liên kết với nhau ra sao.

## 1. Nguyên tắc

Một project software đầy đủ phải xem xét cả business knowledge, common application design và business-specific design.

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
  │   ├── Application Architecture
  │   ├── Backend Architecture
  │   ├── Frontend Architecture
  │   ├── API / Pagination / Error conventions
  │   ├── Transaction / Concurrency
  │   └── Authentication / Session
  │
  └── Business / Feature Design
      ├── Domain solution decision
      ├── Data / Database
      ├── API
      ├── Screen / UX
      ├── Batch / Job
      ├── Event / Messaging
      ├── External Interface
      ├── Business Security
      └── Business Configuration
  ↓
Deliverable Inventory + Type Lists
  ↓
Roadmap / Milestone / Task
  ↓
Verification
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
| Common Design | Application architecture | `APP-ARCH-` | Mọi application không-trivial | `40-design/00-common/architecture/*` |
| Common Design | Backend architecture | `BE-ARCH-` | Có backend | `40-design/00-common/backend/*` |
| Common Design | Frontend architecture | `FE-ARCH-` | Có frontend | `40-design/00-common/frontend/*` |
| Common Design | API conventions | `API-STD-` | Có API | `40-design/00-common/api/*` |
| Common Design | Transaction/concurrency | `TX-STD-` | Có write/persistence/job | `40-design/00-common/data/*` |
| Common Design | Authentication/session | `AUTH-DES-` | Có authenticated user | `40-design/00-common/security/*` |
| Business Design | System/domain architecture | `ARCH-` | Mọi system không-trivial | `40-design/architecture/*` |
| Business Design | Design decision | `DES-` | Khi requirement cần quyết định solution | `40-design/DES-*` |
| Business Design | Logical/physical DB design | `DBD-` | Khi có persistence | `40-design/data/*` |
| Business Design | API specification | `API-DES-` | Khi có API deliverable | `40-design/api/*` |
| Business Design | Screen specification | `SCR-DES-` | Khi có UI screen | `40-design/screen/*` |
| Business Design | Job specification | `JOB-DES-` | Khi có scheduled/background processing | `40-design/job/*` |
| Business Design | Event specification | `EVT-DES-` | Khi publish/consume event | `40-design/event/*` |
| Business Design | Interface specification | `INT-DES-` | Khi tích hợp external system/file/protocol | `40-design/integration/*` |
| Business Design | Security design | `SEC-DES-` | Khi có business authorization/audit rule | `40-design/security/*` |
| Business Design | Configuration design | `CFG-DES-` | Khi behavior nghiệp vụ configurable | `40-design/configuration/*` |
| Deliverable | Output entity | `SCR-/API-/EVT-/JOB-/DB-/INT-/FILE-/MAIL-/NOTI-` | Derived từ design | `50-deliverables/*` |
| Inventory | Screen list | derived | Có screen | `50-deliverables/00-lists/SCREEN-LIST.md` |
| Inventory | API list | derived | Có API | `50-deliverables/00-lists/API-LIST.md` |
| Inventory | Batch/Job list | derived | Có batch/job | `50-deliverables/00-lists/BATCH-JOB-LIST.md` |
| Inventory | Interface list | derived | Có system interface | `50-deliverables/00-lists/INTERFACE-LIST.md` |
| Inventory | Event list | derived | Có event | `50-deliverables/00-lists/EVENT-LIST.md` |
| Inventory | Database list | derived | Có DB/data store | `50-deliverables/00-lists/DATABASE-LIST.md` |
| Inventory | File list | derived | Có file input/output | `50-deliverables/00-lists/FILE-LIST.md` |
| Inventory | Mail list | derived | Có email output | `50-deliverables/00-lists/MAIL-LIST.md` |
| Inventory | Notification list | derived | Có notification | `50-deliverables/00-lists/NOTIFICATION-LIST.md` |
| Planning | Roadmap / milestone | `ROADMAP-`, `MS-` | Project triển khai theo phase/release | `55-planning/*` |
| Execution | Task | `TASK-` | Khi có work phải thực thi | `60-tasks/*` |
| Verification | Test strategy | `TEST-STRAT-` | Mọi project production | `70-verification/TEST-STRATEGY.md` |
| Verification | Verification definition | `TEST-`, `PERF-`, `SEC-TEST-` | Theo requirement/deliverable | `70-verification/*` |
| Operations | Deployment design | `DEP-` | Có deployable runtime | `80-operations/*` |
| Operations | Observability / runbook | `OBS-`, `RUN-` | Production system | `80-operations/*` |
| Change | Change request | `CR-` | Baseline object cần thay đổi | `85-change-management/*` |
| Traceability | Coverage / impact view | derived | Mọi governed project | `90-traceability/*` |

## 3. Common Design vs Business Design

Common Design là baseline kỹ thuật có phạm vi Project/Module/DeliverableType. Business Design kế thừa baseline đó và chỉ mô tả solution đặc thù. Ví dụ `API-DES-001` không cần định nghĩa lại pagination/error format vì mặc định kế thừa `API-STD-001`; `JOB-DES-001` kế thừa transaction/concurrency rule từ `TX-STD-001`.

Một thay đổi Common Design phải được coi là change có blast radius lớn và impact-analysis theo `appliesTo`.

## 4. Inventory list vs detailed specification

List document không thay thế design spec. Nó là **index/coverage view** để trả lời: project hiện có bao nhiêu screen, batch, interface, file, mail, notification; mỗi output có ID gì; design/task/test/status ở đâu.

```text
SCREEN-LIST
   ↓ points-to
SCR-ORD-001
   ↑ specified-by
SCR-DES-001
```

Trong SaaS thật, các list này nên generate từ Deliverable registry thay vì nhập tay.

## 5. Applicability rule

Catalog là checklist có điều kiện, không phải bureaucracy bắt buộc.

```text
Not Evaluated → Applicable → Draft → Baseline
              ↘ Not Applicable (reason required)
```

Nếu project không có email/file/batch thì corresponding inventory type vẫn được evaluate nhưng có thể là `Not Applicable`, không cần tạo design/spec rỗng.

## 6. Coverage rule

Một project chỉ được coi là design-ready khi:

1. Mọi Functional Requirement có Acceptance Criteria.
2. NFR categories đã được review.
3. Common Design baseline áp dụng cho project đã được baseline hoặc explicitly waived.
4. Data ownership, retention và audit requirement rõ nếu có persistence.
5. External integration có contract/error/retry/idempotency requirement.
6. Mọi planned deliverable có design product phù hợp với type của nó.
7. Inventory lists không có deliverable mồ côi: thiếu requirement/design/task/verification phải được flag.
8. Verification strategy cover cả functional lẫn NFR quan trọng.
9. Deployment/operations readiness được xem xét trước production release.

## 7. Traceability không phải cây folder

Folder chỉ giúp con người duyệt tài liệu. Quan hệ thật là graph. Common design có thể constrain hàng loạt business design/deliverable, trong khi một NFR có thể constrain API/DB/screen/job cùng lúc.

Project Template của SaaS nên dùng catalog này để sinh checklist, output inventory và coverage report tự động.