# Software Project Governance SaaS — Product Requirements

## 1. Product Definition

Sản phẩm là một **SaaS quản lý dự án phần mềm** tập trung vào việc quản lý có cấu trúc toàn bộ chuỗi từ mục tiêu của project đến requirement, design, deliverable/output, task, implementation artifact, verification và change impact.

Sản phẩm không thay thế hoàn toàn source control, CI/CD hay issue tracker. Giá trị cốt lõi của nó là tạo ra một **system of record cho project knowledge + planned outputs + execution + traceability**, sau đó tích hợp với repository, CI và AI/human workers bên ngoài.

Mỗi user SaaS có thể tạo và tham gia nhiều project. Mỗi project có cấu trúc document được khởi tạo từ template. Các object quan trọng trong project đều có ID ổn định và liên kết với nhau bằng relation có cấu trúc, để có thể drill-down từ business goal đến task và trace ngược từ task/output lên requirement/goal.

```text
User / Account
   ↓
Projects
   ↓
Project Template
   ↓
Goal / Scope
   ↓
Requirement / Rule / Acceptance Criterion
   ↓
Design Decision / Specification
   ↓
Deliverable / Output
   ↓
Task
   ↓
Implementation Artifact
   ↓
Verification / Evidence
```

AI không phải domain trung tâm. Human, AI Agent và Service Account là các actor có thể tham gia project thông qua permission và API. AI có thể đọc context của task, cập nhật trạng thái, submit result hoặc artifact, nhưng không được có schema/task lifecycle riêng chỉ vì nó là AI.

## 2. SaaS và tenancy

### 2.1 User

`User` là tài khoản đăng nhập vào SaaS. Một user có thể:

- tạo nhiều project;
- tham gia project do user khác tạo;
- có role khác nhau ở từng project;
- sở hữu hoặc quản trị project;
- tạo credential/integration theo quyền được cấp.

### 2.2 Project

`Project` là boundary chính của dữ liệu và governance. Document, knowledge object, deliverable, task, relation, milestone, change request, verification và project member đều thuộc một project.

Mọi API và query phải enforce project boundary. Không được để ID của project A có thể được dùng để đọc/sửa object của project B chỉ vì caller biết ID đó.

### 2.3 Project Member

Project member không đồng nghĩa với SaaS user. Một project có thể chứa ba loại principal:

```text
Project Member
├── Human Member  → liên kết tới SaaS User
├── AI Agent      → machine principal
└── Service       → machine principal / integration
```

Task chỉ biết assignee là một `ProjectMember`. Việc member là human hay AI không làm thay đổi core task model.

## 3. Project Template và Document Template

### 3.1 Project Template

Khi tạo project, user có thể chọn một `ProjectTemplate`. Template xác định baseline structure của project, ví dụ:

```text
Software Project Standard
├── 00 Project Governance
├── 10 Goals & Scope
├── 20 Requirements
├── 30 Architecture & Design
├── 40 Deliverables
├── 50 Planning & Tasks
├── 60 Verification
└── 70 Change Management
```

Project template có thể định nghĩa:

- folder/document tree mặc định;
- document templates bắt buộc hoặc tùy chọn;
- object types được phép;
- ID prefix/rule;
- relation vocabulary;
- lifecycle policy;
- required design products theo deliverable type;
- Definition of Ready / Done policy;
- validation rules;
- role/permission presets;
- export layout.

Template chỉ là điểm khởi tạo và policy definition. Sau khi project được tạo, project giữ reference tới template version đã dùng để đảm bảo reproducibility.

### 3.2 Document Template

Mỗi document có thể được tạo từ template, ví dụ:

- Project Charter;
- Goal & Scope;
- Functional Requirements;
- Non-functional Requirements;
- Architecture Overview;
- System Design;
- Screen Specification;
- API Specification;
- Batch/Job Specification;
- Data Specification;
- Test Strategy;
- Change Request;
- Release/Milestone Plan.

Document template phải có thể chứa structured placeholders hoặc section definitions, không chỉ là đoạn Markdown copy sẵn.

Ví dụ template API Specification có thể yêu cầu:

```text
Overview
Related Requirements
Request Contract
Response Contract
Authorization
Validation Rules
Error Cases
Idempotency
Observability
Related Deliverable
Verification Requirements
```

### 3.3 Document và semantic object phải tách biệt

Document là container phục vụ authoring/navigation. Goal, Requirement, Design Decision, Deliverable hay Acceptance Criterion là semantic objects có ID và lifecycle riêng.

Một document có thể chứa nhiều object. Một object có thể được reference/render ở nhiều document mà không duplicate canonical metadata.

## 4. ID và Identity Model

Mọi object có thể tham gia planning hoặc traceability phải có hai loại identity:

1. `id`: immutable technical ID, ví dụ UUID/ULID, dùng trong database/API.
2. `key`: human-readable project-scoped key, dùng trong tài liệu và giao tiếp.

Ví dụ:

```text
GOAL-001
BF-P2P-001
REQ-P2P-012
BR-P2P-006
AC-P2P-012-01
DES-P2P-005
API-P2P-007
SCR-P2P-003
TASK-P2P-BE-042
VER-P2P-012
CR-2026-004
```

Key phải unique trong project và có thể được sinh theo rule của template. Khi object đã baseline/reference rộng rãi, key không nên bị đổi tùy ý; rename title không làm đổi identity.

## 5. Drill-down model: Goal đến Task

Ứng dụng phải cung cấp một hierarchy view để người dùng có thể drill-down, nhưng canonical model là graph chứ không ép mọi thứ vào một cây duy nhất.

Một flow điển hình:

```text
Goal
  ↓ decomposes-to
Business Capability / Flow
  ↓ decomposes-to
Requirement
  ├── governed-by → Business Rule
  └── accepted-by → Acceptance Criterion
  ↓ satisfied-by
Design Decision
  ↓ introduces
Deliverable
  ↓ specified-by
Design Specification
  ↓ implemented-by
Task
```

Task là đơn vị execution thấp nhất trong planning model. Source files, commits, PRs hay CI runs là implementation/evidence objects, không phải task con bắt buộc.

Ứng dụng phải cho phép:

- drill-down từ Goal xuống Task;
- trace-up từ Task lên Goal;
- xem coverage tại từng tầng;
- phát hiện object bị orphan;
- phát hiện requirement chưa có design/output/task;
- phát hiện task không có upstream reason.

## 6. Core Functional Areas

### 6.1 Project Management

- Create/archive project.
- Project settings.
- Project template selection.
- Project member management.
- Role/permission.
- Activity/audit history.

### 6.2 Document & Knowledge Management

- Document tree.
- Create document from template.
- Rich text/Markdown authoring.
- Structured object insertion/reference.
- Version history.
- Baseline/versioning.
- Search by text, ID, type, status, owner.
- Backlink/reference view.

Structured object types ban đầu:

- Goal.
- Business Capability.
- Business Flow.
- Requirement.
- Business Rule.
- Acceptance Criterion.
- Design Decision.
- Design Specification.
- Architecture Rule.
- Data Specification.
- Interface Contract.
- Policy/Standard.

### 6.3 Deliverable / Output Management

Deliverable là output mà project quyết định phải tồn tại.

Types ban đầu:

- Screen.
- API.
- Batch/Job.
- Event.
- File.
- Report.
- Notification.
- Interface.
- Data Object.
- Database Object.
- Configuration.
- Deployment Artifact.
- Documentation Deliverable.

Mỗi deliverable phải có ID/key, type, name, owner, lifecycle, version, upstream requirement/design, specification coverage, implementing task và verification coverage.

### 6.4 Task & Planning

- Task CRUD.
- Task dependency.
- Assignee.
- Priority.
- Phase/Milestone/Roadmap.
- Read set / required context.
- Write scope / target deliverables.
- Acceptance criteria.
- Verification requirements.
- Result/evidence.
- Kanban/list/dependency views.

Task lifecycle:

```text
Draft → Ready → In Progress → Review → Done
          ↕          ↕
        Blocked    Blocked
```

`Cancelled` là terminal state khác `Done`.

### 6.5 Traceability Graph

Relation là first-class data, không chỉ là hyperlink trong text.

Vocabulary ban đầu:

```text
decomposes-to
accepted-by
governed-by
satisfied-by
introduces
specifies
implements
produces
realizes
verifies
depends-on
owned-by
assigned-to
contained-in
supersedes
impacts
```

Reverse relation được query/generated, không lưu một bản editable thứ hai.

### 6.6 Verification & Evidence

Phải tách:

```text
Verification Definition
   ↓ executed-as
Verification Run
   ↓ produces
Evidence
```

Một test definition không tự chứng minh output đã pass. Verification run phải gắn với revision cụ thể.

### 6.7 Change Management

Mọi thay đổi có khả năng ảnh hưởng baseline phải có thể được quản lý như một change set/request.

Chi tiết ở mục 10.

### 6.8 Integration & Synchronization

Ứng dụng phải API-first và có khả năng export/import/sync project data với external project folder, source repository, CI/CD và AI/service clients.

Chi tiết ở mục 9.

## 7. Lifecycle và Baseline

### 7.1 Knowledge Object

```text
Draft → In Review → Baseline → Superseded
             ↘ Rejected
```

Object đã Baseline không được âm thầm sửa tại chỗ. Edit tạo revision/version mới và có thể kích hoạt impact analysis.

### 7.2 Deliverable

```text
Planned → Specified → Implemented → Verified → Accepted → Deprecated
```

### 7.3 Document

Document có version riêng. Thay đổi wording/layout không nhất thiết làm thay đổi semantic version của mọi object bên trong.

### 7.4 Project Snapshot/Baseline

Hệ thống nên có `ProjectBaseline` hoặc `Snapshot` để cố định tập version của knowledge/deliverable tại một thời điểm, phục vụ release, audit và export reproducible.

## 8. External Project Folder Export

### 8.1 App là canonical source cho governance data

Ở MVP, dữ liệu có cấu trúc trong SaaS là source of truth. Project folder là projection/export để developer, tooling và AI có thể làm việc thuận tiện.

Không được coi cả database SaaS và file local là hai canonical sources độc lập nếu chưa có conflict-resolution protocol.

### 8.2 Export bundle

User có thể export project hoặc một scope của project thành bundle machine-readable + human-readable, ví dụ:

```text
.project-governance/
  manifest.yaml
  project.yaml
  relations.yaml
  baseline.yaml
  documents/
    10-goals/
    20-requirements/
    30-design/
    40-deliverables/
    50-tasks/
  objects/
    goals.yaml
    requirements.yaml
    deliverables.yaml
    tasks.yaml
```

Human-readable Markdown có thể chứa front matter:

```yaml
---
key: REQ-P2P-012
objectId: 01J...
version: 4
baseline: BL-2026-09-001
---
```

`manifest.yaml` phải ghi ít nhất:

- project identity;
- export/snapshot ID;
- generatedAt;
- schema version;
- included object IDs/versions;
- checksum/hash cần thiết;
- source SaaS project reference.

Nhờ vậy AI hoặc tool trong local repository có thể biết chính xác tài liệu nào và version nào đang được dùng.

### 8.3 Import/sync về sau

MVP có thể bắt đầu với one-way export từ app. Two-way sync chỉ được bật khi có:

- identity preservation;
- version comparison;
- optimistic concurrency;
- conflict detection;
- change set generation;
- explicit resolution policy.

Không được silently overwrite SaaS baseline bằng một file local cũ hơn.

## 9. External API và Machine Authentication

### 9.1 API-first

Mọi chức năng quan trọng phải có API tương ứng để UI, CLI, AI agent và integrations dùng cùng application layer.

Các API use case tối thiểu:

```text
GET  project/task context
GET  object/document/deliverable by key
GET  relations / impact graph
POST task status transition
POST task result
POST implementation artifact
POST verification run/evidence
POST change request
POST sync/import proposal
```

### 9.2 Agent/Service authentication

AI agent không nên dùng credential của human user. Nó phải có machine identity riêng.

Model đề xuất:

```text
Machine Principal
  ├── AI Agent
  └── Service Account
        ↓
Credential
        ↓
Project Membership + Role + Scopes
```

MVP có thể dùng scoped API token:

- token chỉ hiện plaintext một lần khi tạo;
- server chỉ lưu hash;
- token có expiry;
- có revoke/rotate;
- token gắn với machine principal;
- token bị giới hạn theo project và scopes;
- mọi action có audit actor rõ ràng.

Scopes ví dụ:

```text
project:read
document:read
object:read
task:read
task:update-status
task:submit-result
artifact:create
verification:submit
change:create
```

AI agent mặc định không có quyền baseline requirement/design, manage member hoặc accept deliverable.

Về sau có thể hỗ trợ OAuth2 Client Credentials, OIDC workload identity hoặc signed short-lived tokens; core authorization model không phụ thuộc cơ chế credential cụ thể.

### 9.3 Task Context API

Một API quan trọng:

```text
GET /api/v1/projects/{projectKey}/tasks/{taskKey}/context
```

Response phải resolve được:

- task metadata;
- required input objects và exact versions;
- related documents;
- target deliverables;
- allowed scope;
- acceptance criteria;
- verification requirements;
- dependency state;
- current baseline/snapshot.

AI không cần crawl toàn bộ project để đoán context.

### 9.4 Task update protocol

AI/human tool bên ngoài có thể transition task thông qua command API thay vì patch raw status:

```text
POST /tasks/{taskKey}/transitions
{
  "transition": "start",
  "expectedVersion": 7
}
```

Ứng dụng validate lifecycle, permission và optimistic concurrency trước khi thay đổi.

Task result submission phải hỗ trợ idempotency key để retry an toàn.

## 10. Change Management và Impact Analysis

### 10.1 Change Request / Change Set

Khi requirement, design, deliverable contract hoặc baseline object cần thay đổi, hệ thống tạo `ChangeRequest`/`ChangeSet` thay vì chỉ edit rồi mất lịch sử.

```text
Change Request
- key
- title
- reason
- source / trigger
- proposedChanges[]
- affectedObjects[]
- impactAssessment
- owner
- status
- decision
```

Lifecycle tham khảo:

```text
Draft → Impact Analysis → Review → Approved → Applying → Verified → Closed
                         ↘ Rejected
```

### 10.2 Impact traversal

Khi một baseline object đổi version:

```text
Changed Object
   ↓
Traceability Graph Traversal
   ↓
Potentially Impacted Objects
   ├── Documents
   ├── Requirements / Rules
   ├── Design Specifications
   ├── Deliverables
   ├── Tasks
   ├── Tests / Verification
   └── Milestones / Releases
```

Relation type phải có metadata cho biết thay đổi có propagate impact hay không và theo hướng nào.

### 10.3 Impact disposition

Không phải downstream object nào cũng bắt buộc sửa. Mỗi impact item phải được disposition:

```text
Update Required
Review Required
Revalidation Required
Replan Required
No Change Required
Obsolete
```

Người xử lý phải ghi rationale. Hệ thống giữ audit trail để biết tại sao object bị sửa hoặc không sửa.

### 10.4 Staleness

Nếu `API-P2P-007-SPEC v3` được baseline nhưng task/verification vẫn dựa trên v2, hệ thống phải có khả năng đánh dấu relation/input là stale và yêu cầu review/revalidation theo policy.

### 10.5 Change application

Approved change có thể:

- tạo version mới của document/knowledge object;
- tạo/sửa deliverable version;
- tạo task mới hoặc reopen/replan task liên quan;
- invalidate/revalidate verification;
- update milestone scope;
- tạo export snapshot mới.

Không xóa lịch sử version cũ.

## 11. Key Screens

### SCR-001 SaaS Home

Danh sách project user sở hữu/tham gia, recent activity và project health summary.

### SCR-002 Create Project

Chọn project template, name/key, visibility và initial members.

### SCR-003 Project Overview

Requirement coverage, deliverable health, task progress, change requests, stale objects, blocked work, recent baseline và verification failures.

### SCR-004 Document Explorer

Tree navigation, template-based creation, structured object placement, version/history/backlinks.

### SCR-005 Goal → Task Explorer

Drill-down hierarchy/graph từ Goal đến Requirement, Design, Deliverable và Task; hỗ trợ reverse trace.

### SCR-006 Deliverable Inventory / Detail

Theo dõi output, lifecycle, version, upstream reason, specs, implementing tasks, artifacts và verification.

### SCR-007 Task Board / Task Detail

Planning/execution, assignee, context, scope, dependency, result và external updates.

### SCR-008 Traceability Graph / Matrix

Graph + table views, filter theo relation/type/status/version.

### SCR-009 Roadmap / Milestone

Phase → milestone → outcomes/tasks, progress theo deliverable outcome.

### SCR-010 Change Center

Change requests, proposed diffs, impact graph, disposition, applying status và revalidation.

### SCR-011 Integration & API Access

Repository binding, export settings, machine principals, token/scopes, webhooks và sync status.

### SCR-012 Verification Center

Verification definitions, runs, evidence, stale verification và failures.

## 12. Validation Rules

Tối thiểu:

1. Human-readable key unique trong project.
2. Technical ID immutable.
3. Cross-project reference bị cấm trừ loại relation được thiết kế explicit cho shared assets sau này.
4. Relation source/target phải đúng schema.
5. Không lưu reverse relation editable riêng.
6. Baseline object edit phải tạo version mới.
7. Task không thể Ready nếu mandatory context/output/acceptance/dependency chưa đạt policy.
8. Deliverable không thể Specified nếu mandatory design products còn thiếu.
9. Deliverable không thể Verified nếu required verification chưa pass trên version/revision hiện tại.
10. Dependency graph được khai báo acyclic không được có cycle.
11. External status update phải đi qua transition command và permission check.
12. Machine credential phải scoped, revocable và auditable.
13. Import/sync không được overwrite version mới hơn mà không có conflict resolution.
14. Baseline change phải tạo impact analysis nếu relation policy yêu cầu.
15. Task result phải trace được người/machine submit, task version và project revision/baseline liên quan.

## 13. MVP Roadmap

### MVP-1 — SaaS Foundation

- User authentication.
- Multi-project per user.
- Project membership/RBAC.
- Project template.
- Document tree + document template.
- Structured object IDs/version.
- Basic search.

### MVP-2 — Software Project Governance

- Goal → Requirement → Design → Deliverable → Task model.
- Deliverable inventory.
- Task board.
- Relation graph.
- Traceability matrix.
- Validation/coverage.
- Milestone/roadmap.

### MVP-3 — Change & Baseline

- Baseline/snapshot.
- Object diff/version history.
- Change request/change set.
- Impact analysis.
- Stale/revalidation handling.

### MVP-4 — External Integration

- Export project governance bundle to folder/repository.
- GitHub repository binding.
- Implementation artifact links.
- Verification run/evidence ingestion.
- Webhook/event ingestion where useful.

### MVP-5 — AI / Machine Worker

- Machine principal.
- Scoped API token.
- Task context API.
- Task transition/result API.
- Artifact/evidence submission.
- Agent activity/audit.

AI support nằm sau core project model; không có AI thì sản phẩm vẫn là một SaaS quản lý dự án phần mềm hoàn chỉnh theo hướng traceability-first.

## 14. Non-functional Requirements

- API-first.
- Multi-tenant security enforced server-side.
- Audit trail cho permission, baseline, status, relation, change và machine action.
- Optimistic concurrency cho update/version/transition.
- Idempotency cho command từ external agents/services.
- Full-text search + structured filter.
- Project có hàng chục nghìn objects vẫn phải traversal/query thực dụng.
- Export/import có schema version.
- Credential secret phải được hash/encrypt phù hợp; plaintext token không được lưu sau khi phát hành.
- External integration outage không làm mất khả năng quản lý core project data.
- PostgreSQL là canonical persistence cho MVP; graph có thể dùng edge table + recursive query trước khi cân nhắc graph database.

## 15. Product Success Criteria

Sản phẩm đạt mục tiêu khi user có thể mở một project và trả lời được bằng dữ liệu có cấu trúc:

- Project này đang nhằm đạt Goal nào?
- Goal đó được phân rã thành requirement nào?
- Requirement nào chưa có design hoặc deliverable?
- Những output nào project phải tạo?
- Output nào chưa được implement/verify?
- Task nào tạo hoặc thay đổi output đó?
- Task cần đọc tài liệu/object/version nào?
- Human/AI/service nào đang chịu trách nhiệm?
- Source code/PR/artifact nào hiện thực deliverable?
- Nếu requirement hoặc API contract thay đổi thì document, deliverable, task, test và milestone nào bị ảnh hưởng?
- Những impact nào đã được xử lý và vì sao?
- Có thể export một snapshot nhất quán của project vào repository để human/AI làm việc không?
- External AI/service có thể cập nhật task/result an toàn qua API mà không cần dùng credential của user không?

Nguyên tắc trung tâm:

> **SaaS là system of record cho software project knowledge, output, work, change và traceability. Repository, CI, human và AI là các môi trường/actor được tích hợp xung quanh core đó, không phải các source of truth cạnh tranh không kiểm soát.**
