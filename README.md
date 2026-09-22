# Software Project Governance SaaS

Repository này thiết kế một **SaaS quản lý dự án phần mềm theo hướng traceability-first**. Mỗi user có thể tạo hoặc tham gia nhiều project. Trong mỗi project, hệ thống quản lý document, goal, requirement, design, deliverable/output, task, dependency, milestone, verification, change và liên kết giữa tất cả các đối tượng đó bằng ID ổn định.

Trọng tâm của sản phẩm là trả lời được toàn bộ chuỗi:

```text
Goal
  ↓
Business Flow / Capability
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

User phải có thể drill-down từ Goal đến Task và trace ngược từ một task, API, screen, source artifact hoặc verification result về lý do business ban đầu của nó.

## 1. SaaS model

```text
User
 ├── Project A
 ├── Project B
 └── Project C

Project
 ├── Members
 ├── Documents & Knowledge
 ├── Deliverables
 ├── Tasks & Roadmap
 ├── Relations / Traceability
 ├── Changes / Impact
 ├── Verification
 └── Integrations
```

`User` là tài khoản SaaS toàn cục. Actor trong project là `ProjectMember`/`Principal`:

```text
Principal
├── Human
├── AI Agent
└── Service Account
```

AI chỉ là một member có machine identity, permission và API credential riêng. Task model không thay đổi tùy assignee là human hay AI.

## 2. Project Template và Document Template

Khi tạo project, user chọn một `ProjectTemplate`. Template có thể định nghĩa document tree, ID rules, relation vocabulary, lifecycle, required design products, validation policy, role presets và export layout.

Ví dụ structure mặc định:

```text
00 Project Governance
10 Goals & Scope
20 Requirements
30 Architecture & Design
40 Deliverables
50 Planning & Tasks
60 Verification
70 Change Management
```

Các document trong project có thể được tạo từ template như Project Charter, Requirement Specification, API Specification, Screen Specification, Batch Specification, Data Specification, Test Strategy và Change Request.

Document là container authoring; Requirement, Design Decision, Deliverable... là structured semantic objects có ID/version riêng.

## 3. Identity và traceability

Mỗi object quan trọng có:

```text
Technical ID  → immutable UUID/ULID
Human Key     → project-scoped stable ID
```

Ví dụ:

```text
GOAL-001
REQ-P2P-012
DES-P2P-005
API-P2P-007
TASK-P2P-BE-042
CR-2026-004
```

Relation là first-class data, không chỉ là hyperlink trong Markdown.

```text
REQ-P2P-012
   ↓ satisfied-by
DES-P2P-005
   ↓ introduces
API-P2P-007
   ↓ implemented-by
TASK-P2P-BE-042
```

Reverse relations được query/generated, không lưu một bản editable thứ hai.

## 4. External integration

SaaS là canonical source of truth cho **governance/semantic project state**. Source repository vẫn là source of truth cho code, CI provider cho execution result.

```text
SaaS
  ↓ export snapshot
.project-governance/ in repository
  ↓ human / AI / tooling
Source code / PR / CI
  ↓ API / webhook
SaaS artifact, task result, verification, change data
```

Project có thể export một bundle machine-readable + human-readable vào local/repository folder với `manifest.yaml`, object IDs, versions và checksums.

AI Agent hoặc Service Account không dùng credential của user. Chúng authenticate bằng machine credential có project scope và permission cụ thể, ví dụ `task:read`, `task:update-status`, `task:submit-result`, `artifact:create`.

## 5. Change management

Baseline object không được sửa âm thầm tại chỗ. Khi Requirement, Design, API contract hoặc object quan trọng thay đổi, hệ thống phải tạo version mới và có khả năng quản lý `ChangeRequest`/`ChangeSet`.

```text
Changed Object
   ↓
Impact Analysis
   ↓
Documents / Requirements / Design / Deliverables / Tasks / Tests / Milestones
   ↓
Impact disposition
   ├── Update Required
   ├── Review Required
   ├── Revalidation Required
   ├── Replan Required
   └── No Change Required
```

Hệ thống phải giữ được lịch sử: thay đổi gì, vì sao, ai approve, object nào bị ảnh hưởng, object nào thực sự được sửa và verification nào phải chạy lại.

## 6. Core lifecycle

Knowledge object:

```text
Draft → In Review → Baseline → Superseded
```

Deliverable:

```text
Planned → Specified → Implemented → Verified → Accepted → Deprecated
```

Task:

```text
Draft → Ready → In Progress → Review → Done
```

Task và Deliverable có lifecycle độc lập. `Task Done` không đồng nghĩa `Deliverable Accepted`.

## 7. Documentation

### [APP-REQUIREMENTS.md](./APP-REQUIREMENTS.md)

Canonical product requirements: SaaS tenancy, project/document templates, Goal→Task drill-down, ID/traceability, deliverables, planning, machine API/authentication, change impact, screens, validation và MVP roadmap.

### [APP-DOMAIN-MODEL.md](./APP-DOMAIN-MODEL.md)

Canonical domain model: User, Principal, ProjectMembership, Project, Template, Document, KnowledgeObject, Deliverable, Task, Relation, Baseline, ChangeRequest, Verification, Integration, MachineCredential và Audit.

### [INTEGRATION-SYNC-CHANGE-MANAGEMENT.md](./INTEGRATION-SYNC-CHANGE-MANAGEMENT.md)

Chi tiết protocol export project folder, repository binding, task context API, agent/service authentication, external artifact ingestion, two-way sync, conflict detection và change/impact management.

### [DESIGN-DELIVERABLE-TASK-GOVERNANCE.md](./DESIGN-DELIVERABLE-TASK-GOVERNANCE.md)

Tài liệu nền về design/deliverable/task governance. Nội dung này sẽ tiếp tục được chuẩn hóa theo canonical SaaS/domain model phía trên.

## 8. Technical direction

MVP thực dụng:

```text
React + TypeScript
        ↓
ASP.NET Core API
        ↓
Modular Application / Domain
        ↓
PostgreSQL
```

PostgreSQL lưu canonical state và relation edges. Recursive CTE/indexes đủ cho MVP trước khi cân nhắc graph database.

External systems đi qua adapters/API:

```text
Core SaaS
  ├── Repository Adapter
  ├── CI Adapter
  ├── Export/Sync Engine
  ├── Webhook Ingestion
  └── AI / Service API
```

## 9. Product principle

> **Đây là một phần mềm quản lý dự án software, trong đó document, requirement, design, output, task, change và verification tạo thành một graph có ID/version/traceability. Human và AI chỉ là các actor làm việc trên graph đó; SaaS mới là nơi quản lý trạng thái, governance và lịch sử thay đổi của project.**
