# Engineering Governance Platform — Product Requirements

## 1. Mục tiêu sản phẩm

Sản phẩm là một ứng dụng quản trị dự án kỹ thuật tập trung vào ba đối tượng cốt lõi: **knowledge/document**, **work/task** và **deliverable/output**. Ứng dụng phải cho phép một team nhìn thấy một cách có cấu trúc dự án đang định nghĩa điều gì, đang làm việc gì, sẽ tạo ra những output nào, ai chịu trách nhiệm, các đối tượng phụ thuộc lẫn nhau ra sao và bằng chứng nào chứng minh một output đã hoàn thành.

AI không phải trung tâm của sản phẩm. AI chỉ là một loại member có thể được giao task, đọc document được cấp quyền, tạo hoặc sửa output, gửi kết quả để review và thực hiện lại khi verification không đạt. Mọi workflow cốt lõi phải hoạt động đầy đủ với team chỉ gồm con người; AI là một execution member có thể được thêm vào sau.

Sản phẩm cần giải quyết vấn đề phổ biến của các project lớn: requirement nằm trong một nhóm Markdown, design nằm ở nơi khác, task nằm trong issue tracker, output thực tế nằm trong source code, còn dependency và traceability tồn tại chủ yếu trong trí nhớ của con người. Ứng dụng phải biến các đối tượng này thành một graph có cấu trúc và lifecycle rõ ràng.

## 2. Nguyên tắc sản phẩm

### 2.1 Project là aggregate cấp cao nhất

Mọi document, requirement, design object, deliverable, task, milestone, verification và member đều thuộc hoặc được liên kết vào một Project. Project là boundary quản trị, phân quyền, versioning và reporting.

### 2.2 AI là Member, không phải workflow trung tâm

Mô hình membership tối thiểu:

```text
Member
├── Human
├── AI Agent
└── Service Account
```

Tất cả member có thể được cấp role và permission. Một AI member có thể có thêm metadata như model/provider, execution endpoint hoặc capability, nhưng task model không được phụ thuộc vào việc assignee là AI hay human.

```text
Task
 ├── assignedTo → Human Member
 └── assignedTo → AI Member
```

Task lifecycle, acceptance criteria, dependency và deliverable relation phải giữ nguyên trong cả hai trường hợp.

### 2.3 Document không đồng nghĩa với domain object

Document là container để con người đọc và viết. Requirement, Business Rule, Design Decision, Screen Specification, API Specification hoặc Acceptance Criteria là các **structured knowledge objects** có ID, type, lifecycle và relation riêng.

Một document có thể chứa nhiều knowledge object; một knowledge object cũng có thể được render hoặc reference ở nhiều document view. Ứng dụng không được phụ thuộc vào giả định “một file Markdown = một requirement/design”.

### 2.4 Deliverable khác Implementation Artifact

Deliverable mô tả thứ project cần tạo hoặc duy trì, ví dụ một Screen, API, Batch, Report, Event, Database Object hoặc Configuration. Implementation Artifact là bằng chứng vật lý/technical hiện thực deliverable đó, ví dụ source file, migration file, OpenAPI document, workflow file hoặc deployment artifact.

```text
Requirement
    ↓
Design Decision
    ↓
Deliverable
    ↓
Task
    ↓
Implementation Artifact
    ↓
Verification
```

### 2.5 Relation là first-class data

Traceability không được chỉ tồn tại dưới dạng hyperlink trong prose. Các relation như `satisfies`, `specifies`, `implements`, `verifies`, `depends-on`, `owned-by`, `assigned-to`, `contained-in` phải được quản lý như dữ liệu có cấu trúc.

### 2.6 Một relation chỉ có một source of truth

Nếu graph lưu edge `TASK-001 implements API-003`, hệ thống không được yêu cầu lưu thêm một bản editable `API-003 implementedBy TASK-001`. Reverse relation phải được query hoặc materialize tự động.

## 3. Phạm vi domain

Ứng dụng được chia thành bảy bounded functional areas.

### 3.1 Project & Membership

Quản lý Project, team member, role, permission, ownership và project settings. Member có thể là Human, AI Agent hoặc Service Account. Permission phải xác định ai được xem, sửa, approve, baseline hoặc execute từng loại object.

### 3.2 Knowledge & Document Management

Quản lý cấu trúc tài liệu, folder, document, section, knowledge object và version. Hệ thống phải hỗ trợ hierarchical navigation nhưng không ép knowledge graph phải theo đúng cây folder.

Các knowledge object điển hình gồm:

- Business Goal.
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
- Standard / Policy.

### 3.3 Deliverable Management

Quản lý danh sách những output mà project cần tồn tại. Deliverable là planned system/product output, không phải task.

Các type ban đầu:

- Screen.
- API.
- Batch / Job.
- Event.
- File.
- Report.
- Notification.
- Interface.
- Data Object.
- Database Artifact.
- Configuration.
- Deployment Artifact.
- Documentation Deliverable.

Mỗi deliverable phải có ID, type, name, owner, lifecycle, source requirement/design relation và implementation/verification coverage.

### 3.4 Task & Planning

Quản lý task, dependency, assignment, priority, milestone, phase/release và roadmap. Task mô tả **work to be performed**, không được dùng để định nghĩa lại requirement hoặc design.

Một task cần tối thiểu:

- Objective.
- Assignee/member.
- Status.
- Priority.
- Input context / read set.
- Scope / write set.
- Expected deliverables or deliverable changes.
- Dependency.
- Acceptance criteria.
- Verification requirements.
- Evidence / result.

### 3.5 Traceability Graph

Cho phép query và visualize relation giữa các object. Hệ thống phải hỗ trợ cả downstream và upstream traversal.

Ví dụ:

```text
BF-P2P-001
    ↓ decomposes-to
REQ-P2P-012
    ↓ satisfied-by
DES-P2P-005
    ↓ introduces
API-P2P-007
    ↓ implemented-by
TASK-P2P-BE-042
    ↓ produces
ART-src-128
    ↓ verified-by
TEST-P2P-012
```

Các câu hỏi bắt buộc hệ thống phải trả lời được:

- Requirement này đã có design chưa?
- Design này tạo ra deliverable nào?
- Deliverable nào chưa có task implement?
- Task này cần đọc knowledge object nào?
- Task này được phép thay đổi deliverable nào?
- Nếu API contract này thay đổi thì task/test nào bị ảnh hưởng?
- Milestone còn thiếu output nào?
- Output này tồn tại vì business requirement nào?
- Artifact implementation này được tạo bởi task nào?
- Có object nào orphan không?

### 3.6 Verification & Evidence

Quản lý verification definition và execution evidence như hai khái niệm khác nhau.

```text
Verification Definition
    ↓ executed-as
Verification Run
    ↓ produces
Evidence
```

Ví dụ `IT-P2P-012` là integration test definition; lần chạy GitHub Actions `RUN-20260922-1831` mới là verification run; log, test result và commit SHA là evidence.

### 3.7 Integration

Ứng dụng phải có khả năng liên kết với external systems nhưng không phụ thuộc vào chúng cho core domain. Integration candidates gồm GitHub/GitLab, CI/CD, issue tracker, code quality tool, artifact storage và AI execution service.

## 4. Core object model

### 4.1 Project

```text
Project
- id
- key
- name
- description
- status
- owner
- settings
```

### 4.2 Member

```text
Member
- id
- type: human | ai | service
- displayName
- status
- capabilities[]
- roles[]
```

AI-specific metadata phải ở extension/profile, không làm thay đổi Task schema cốt lõi.

### 4.3 Document

```text
Document
- id
- title
- parent/folder
- format
- status
- currentVersion
- owners[]
```

### 4.4 Knowledge Object

```text
KnowledgeObject
- id
- type
- title
- status
- owner
- sourceDocument
- sourceAnchor
- version
- content
```

### 4.5 Deliverable

```text
Deliverable
- id
- type
- name
- status
- owner
- version
- repository/location metadata
```

### 4.6 Task

```text
Task
- id
- title
- type
- objective
- status
- priority
- assignee
- milestone
- readSet[]
- writeSet[]
- verifySet[]
```

### 4.7 Relation

```text
Relation
- id
- fromObjectId
- relationType
- toObjectId
- status
- metadata
```

### 4.8 Implementation Artifact

```text
ImplementationArtifact
- id
- type
- repository
- path / externalId
- revision
- checksum
```

### 4.9 Verification Run

```text
VerificationRun
- id
- verificationDefinition
- targetRevision
- executedAt
- executor
- status
- evidence[]
```

## 5. Lifecycle

### 5.1 Knowledge object lifecycle

```text
Draft → In Review → Baseline → Superseded
             ↘ Rejected
```

Baseline nghĩa là version được chấp nhận làm input cho planning/execution. Khi object đã baseline bị sửa, hệ thống phải tạo version mới và chạy impact analysis thay vì âm thầm thay nội dung lịch sử.

### 5.2 Deliverable lifecycle

```text
Planned → Specified → Implemented → Verified → Accepted → Deprecated
```

Deliverable được xem là `Specified` khi các design specification bắt buộc đã đủ; `Implemented` khi implementation artifact cần thiết đã tồn tại; `Verified` khi verification requirements pass; `Accepted` khi được đưa vào accepted project/release baseline.

### 5.3 Task lifecycle

```text
Draft → Ready → In Progress → Review → Done
          ↕          ↕
        Blocked    Blocked

Cancelled có thể xảy ra trước Done.
```

Task `Done` không có nghĩa toàn bộ deliverable đã Accepted. Task là work unit; deliverable có lifecycle độc lập.

## 6. Relation model ban đầu

Relation vocabulary phải được kiểm soát, không cho người dùng tùy ý tạo verb mới nếu chưa khai báo schema.

| Relation | From | To | Ý nghĩa |
|---|---|---|---|
| decomposes-to | Business Flow/Capability | Requirement | Phân rã business scope |
| governed-by | Requirement/Deliverable | Business Rule/Policy | Bị chi phối bởi rule |
| accepted-by | Requirement | Acceptance Criterion | Điều kiện chấp nhận requirement |
| satisfied-by | Requirement | Design Decision | Design giải quyết requirement |
| introduces | Design Decision | Deliverable | Design quyết định cần deliverable |
| specifies | Design Specification | Deliverable | Spec mô tả contract của deliverable |
| implements | Task | Deliverable | Task tạo/sửa deliverable |
| produces | Task | Implementation Artifact | Output vật lý của task |
| realizes | Implementation Artifact | Deliverable | Artifact hiện thực deliverable |
| verifies | Verification Definition | Requirement/Deliverable | Verification target |
| executed-as | Verification Definition | Verification Run | Một lần thực thi verification |
| depends-on | Any allowed object | Any allowed object | Dependency có nghĩa được validate theo type |
| owned-by | Object | Member/Team | Ownership |
| assigned-to | Task | Member | Assignee |
| contained-in | Object | Project/Milestone/Document | Quan hệ container |

## 7. Các màn hình chính

### SCR-001 Project Overview

Hiển thị health tổng thể của project: requirement coverage, deliverable status, task progress, blocked work, missing traceability, recent baselines và verification failures.

### SCR-002 Knowledge Explorer

Cho phép duyệt document tree và knowledge objects. User có thể mở một object để xem nội dung, version, relation upstream/downstream, owner và nơi object được reference.

### SCR-003 Document Editor

Cho phép chỉnh document nhưng đồng thời link/insert structured knowledge object. Editor không được biến nội dung Markdown tự do thành source duy nhất của các metadata cần validate.

### SCR-004 Deliverable Inventory

Danh sách toàn bộ deliverable theo type, system/module, owner, lifecycle, specification coverage, implementation coverage và verification coverage.

### SCR-005 Deliverable Detail

Hiển thị requirement nguồn, design decision, specifications, implementing tasks, implementation artifacts, verification definitions/runs và change history.

### SCR-006 Task Board

Kanban/list view theo status, milestone, owner/assignee, type và dependency. Human và AI member xuất hiện như assignee bình thường.

### SCR-007 Task Detail

Hiển thị objective, read set, write set, expected deliverable changes, acceptance criteria, dependency, assignee, execution result, evidence và review history.

### SCR-008 Traceability Graph

Graph view cho phép chọn root object và traversal depth, filter relation type, object type, status hoặc owner. Phải có cả graph visualization và tabular matrix để xử lý project lớn.

### SCR-009 Roadmap / Milestone

Hiển thị Phase → Milestone → Task và milestone outcome dựa trên deliverable set. Milestone progress phải dựa trên outcome/deliverable thay vì chỉ % task completed.

### SCR-010 Member & Permission

Quản lý Human, AI và Service member, role, permission, capability và activity history.

### SCR-011 Verification Center

Hiển thị verification definitions, runs, evidence, failures và deliverable/requirement bị ảnh hưởng.

### SCR-012 Impact Analysis

Khi một baseline object thay đổi, hiển thị downstream graph và danh sách object cần review/revalidate/replan.

## 8. Workflow chính

### WF-001 Define → Plan → Execute → Verify

```text
Create Project
   ↓
Define Business / Requirement
   ↓
Baseline Requirement
   ↓
Create Design Decision
   ↓
Declare Deliverables
   ↓
Create Specifications
   ↓
Create Tasks
   ↓
Assign Human / AI Member
   ↓
Execute
   ↓
Review Output
   ↓
Run Verification
   ↓
Verify Deliverable
   ↓
Accept into Baseline
```

### WF-002 AI execution

AI flow không tạo workflow riêng ở domain level. Nó chỉ là một implementation của task execution:

```text
Task Ready
   ↓
assigned-to AI Member
   ↓
AI receives task context
   ↓
AI produces task result / artifacts
   ↓
Task → Review
   ↓
Human or automated verifier reviews
   ↓
Done / Rework
```

Cùng task đó có thể reassigned cho Human mà không cần migrate schema.

### WF-003 Change impact

```text
Baseline object changed
   ↓
Create new version
   ↓
Traverse dependency/traceability graph
   ↓
Mark impacted objects
   ↓
Create review/revalidation actions
   ↓
Update tasks / deliverables / verification
   ↓
New baseline
```

## 9. Permission model

MVP cần RBAC cấp Project với permission ít nhất:

- View project.
- Edit document.
- Edit structured object.
- Baseline knowledge object.
- Create/edit deliverable.
- Create/edit task.
- Assign task.
- Execute task.
- Submit task result.
- Review task result.
- Accept deliverable.
- Manage verification.
- Manage members.
- Manage project settings.

AI member mặc định không được baseline requirement/design hoặc Accept deliverable trừ khi project owner chủ động cấp quyền. Điều này là policy mặc định, không phải constraint bản chất của Member model.

## 10. Validation rules MVP

Hệ thống cần hỗ trợ machine validation thay vì chỉ hiển thị warning bằng prose. Các rule ban đầu:

1. ID phải unique trong project.
2. Relation type phải hợp lệ với source/target type.
3. Task không thể Ready nếu không có objective, assignee policy, expected output/scope và acceptance/verification requirement phù hợp.
4. Deliverable không thể Specified nếu mandatory spec theo deliverable type còn thiếu.
5. Deliverable không thể Verified nếu required verification chưa có successful run trên revision hiện tại.
6. Baseline object thay đổi phải tạo version mới.
7. Không cho tạo dependency cycle ở các graph được khai báo acyclic.
8. Không cho object reference tới ID không tồn tại.
9. Không lưu reverse relation như dữ liệu editable độc lập.
10. Task Done phải có execution result và relation tới các output/artifact đã tạo hoặc phải có explicit `no-artifact` reason.

## 11. MVP scope

MVP không cần cố build toàn bộ SDLC platform. Phạm vi đầu tiên nên đủ để chứng minh model.

### MVP-1 Foundation

- Project.
- Member.
- Basic RBAC.
- Document tree.
- Knowledge object CRUD/version.
- Deliverable inventory.
- Task CRUD/assignment/status.
- Relation graph.
- Basic validation.

### MVP-2 Planning & Traceability

- Milestone/roadmap.
- Dependency graph.
- Read set / write set / verify set.
- Coverage report.
- Traceability matrix.
- Impact analysis.

### MVP-3 Verification & Repository Integration

- Verification definition/run/evidence.
- GitHub repository integration.
- Link task ↔ commit/PR/file.
- CI result ingestion.
- Deliverable verification coverage.

### MVP-4 AI Member Integration

- Register AI member.
- Capability/profile.
- Task dispatch adapter.
- Context package generated from task read set.
- Result/artifact ingestion.
- Rework after review/verification failure.

AI integration cố ý nằm sau core governance vì app phải có giá trị ngay cả khi không có AI.

## 12. Non-functional requirements

- Web app, desktop-first responsive UI.
- Object ID và relation graph phải query nhanh trên project có hàng chục nghìn object.
- Audit trail cho mọi baseline/status/relation quan trọng.
- Optimistic concurrency hoặc equivalent để tránh ghi đè version.
- Search full-text kết hợp structured filter.
- Export/import machine-readable format cho portability.
- API-first để AI/service integration không phụ thuộc trực tiếp UI.
- Integration failure không được làm mất khả năng quản lý core project data.
- Permission phải được enforce ở API/backend, không chỉ ẩn nút trên frontend.
- Mọi status transition quan trọng phải có validation ở domain/application layer.

## 13. Kiến trúc kỹ thuật đề xuất ban đầu

Với stack quen thuộc có thể dùng:

```text
React + TypeScript
        ↓
ASP.NET Core API
        ↓
Application / Domain
        ↓
PostgreSQL
```

PostgreSQL đủ cho MVP. Relation graph có thể lưu bằng relational table `relations` với indexes phù hợp và recursive CTE; chưa cần graph database ngay từ đầu. Nếu sau này traversal/analytics trở thành bottleneck thì có thể bổ sung graph projection mà không thay đổi canonical domain model.

Các integration như GitHub, CI/CD hoặc AI executor nên đi qua adapter:

```text
Core Application
    ↓ ports
Integration Adapters
    ├── GitHub
    ├── CI/CD
    ├── Quality Tools
    └── AI Executor
```

## 14. Definition of success

Sản phẩm đạt mục tiêu khi một project manager/architect/developer có thể mở ứng dụng và trả lời bằng dữ liệu có cấu trúc, không cần đọc toàn bộ repository:

- Dự án đang định nghĩa những requirement nào?
- Những output nào phải tồn tại?
- Output nào chưa được design/implement/verify?
- Team đang làm task gì và vì sao task đó tồn tại?
- Human hay AI nào đang chịu trách nhiệm?
- Task cần đọc object nào trước khi thực hiện?
- Output thực tế nằm ở đâu?
- Nếu một requirement hoặc contract đổi thì ảnh hưởng gì?
- Release/milestone còn thiếu outcome nào?

Nguyên tắc quan trọng nhất của sản phẩm là:

> **Project knowledge, work và output là domain trung tâm. Human và AI chỉ là những member cùng tham gia làm thay đổi các domain object đó theo permission, lifecycle và verification được hệ thống quản lý.**
