# Software Project Governance SaaS — Product Areas

## 1. Purpose

Tài liệu này là **canonical product decomposition** của sản phẩm. Mọi requirement, domain concept, screen, API, task phát triển sản phẩm và acceptance criteria phải map được về một `Product Area` và một `Capability` cụ thể.

Sản phẩm được chia thành ba Product Area chính theo lifecycle thực tế:

```text
PA-01 Template & Methodology Management
        ↓ instantiate
PA-02 Project Workspace & Traceability
        ↓ execute / collaborate
PA-03 Work Management & Human-AI Collaboration
```

Các capability kỹ thuật dùng chung như identity/RBAC, versioning, audit, API, export/sync được xem là **Platform / Cross-cutting capabilities**, không phải Product Area độc lập.

`docs/sample-project/` chỉ là acceptance fixture để kiểm tra hệ thống có thể biểu diễn một bộ software-project documentation đủ sâu. Nó không phải product requirement source.

---

# 2. PA-01 — Template & Methodology Management

## 2.1 Product question

> Một software project chuẩn phải được tổ chức, mô tả, breakdown, giao việc và kiểm chứng như thế nào?

PA-01 quản lý **metamodel/methodology** dùng để sinh project thật. Template không chứa runtime progress của một project cụ thể.

## 2.2 Capability map

### PA01-C01 — Project Template Management

Quản lý `ProjectTemplate` và version của template:

- create/update/archive/publish template;
- clone/fork template;
- version template;
- define template metadata và compatibility;
- preview template trước khi publish;
- xác định exact template version dùng để instantiate project.

### PA01-C02 — Structure Template Management

Quản lý cây folder/document chuẩn:

```text
ProjectStructureTemplate
├── FolderTemplateNode
├── DocumentTemplateNode
└── FolderTemplateNode
    └── DocumentTemplateNode
```

Phải hỗ trợ create/move/reorder/delete/archive node, required/optional node và validation tree không cycle.

### PA01-C03 — Document Template Management

Quản lý template của từng loại document, ví dụ:

- Requirement Specification;
- Architecture Overview;
- API Specification;
- Screen Specification;
- Batch/Job Specification;
- Data Design;
- Test Strategy;
- Operational Runbook.

`DocumentTemplateVersion` có thể định nghĩa section tree, required/optional sections, placeholder, allowed semantic object types và validation rules.

### PA01-C04 — Semantic Schema & Relation Template

Quản lý loại software-project object và relation vocabulary mà project được phép dùng.

Ví dụ object types:

```text
Goal
BusinessFlow
Requirement
BusinessRule
AcceptanceCriterion
DesignDecision
DesignSpecification
Deliverable
VerificationDefinition
```

Ví dụ canonical relations:

```text
Requirement --requires--> Deliverable
Requirement --satisfied-by--> DesignSpecification
DesignSpecification --specifies--> Deliverable
Task --implements--> Deliverable
VerificationDefinition --verifies--> Requirement / Deliverable
```

Template phải cấu hình được allowed source/target, direction, cycle policy, version sensitivity và impact propagation policy.

### PA01-C05 — Task Template & Tasklist Template

Đây là phần bắt buộc, không được coi task chỉ là runtime object.

Template phải định nghĩa được **task blueprint** và **tasklist blueprint** cho một loại work package/deliverable.

Ví dụ với `API Deliverable`:

```text
API Implementation Tasklist Template
├── T1 Backend implementation
├── T2 Unit test
├── T3 Integration test
└── T4 Review

Dependencies:
T2 depends-on T1
T3 depends-on T1
T4 depends-on T2, T3
```

Task template có thể quy định:

- objective pattern;
- required input/context types;
- target deliverable type;
- required design products;
- allowed write scope;
- acceptance criteria policy;
- verification requirements;
- dependency rules;
- default assignee role/agent type;
- Definition of Ready;
- Definition of Done.

Khi instantiate vào project, task template sinh ra **Task thật**, không dùng template object để lưu runtime status.

### PA01-C06 — Governance & Coverage Policy Template

Template phải định nghĩa được project completeness rules.

Ví dụ:

```text
Requirement is DesignComplete when:
- có AcceptanceCriterion
- có required DesignSpecification
- có required Deliverable links

Deliverable is ImplementationReady when:
- mandatory design products đã baseline
- tasklist đã được tạo
- required dependencies đã resolve
```

Đây là nền tảng để hệ thống trả lời một requirement đã drill-down đủ sâu hay chưa.

### PA01-C07 — Template Versioning & Lifecycle

Lifecycle tối thiểu:

```text
Draft → Published → Deprecated
```

Published template version immutable. Project đã được tạo từ version cũ không bị silently migrate khi template mới được publish.

### PA01-C08 — Template Validation & Preview

Trước khi publish, hệ thống phải kiểm tra tối thiểu:

- structure tree hợp lệ;
- referenced document template version tồn tại;
- required semantic type tồn tại;
- relation vocabulary hợp lệ;
- task dependency không cycle;
- task template input/output có thể resolve;
- coverage/governance rule không reference object type không tồn tại.

---

# 3. PA-02 — Project Workspace & Traceability

## 3.1 Product question

> Trong Project X hiện có những requirement, design, deliverable, document và task nào; chúng liên quan với nhau thế nào; requirement đã drill-down tới đâu và còn thiếu gì?

PA-02 quản lý **runtime project state** được tạo từ template và tiếp tục được chỉnh sửa trong suốt lifecycle project.

## 3.2 Capability map

### PA02-C01 — Project Instantiation

Tạo project từ exact `ProjectTemplateVersion` và sinh:

- project structure tree;
- documents từ document templates;
- enabled semantic object schemas;
- relation policies;
- governance/coverage rules;
- initial tasklist/task blueprints khi template quy định.

Project phải lưu provenance tới template version nhưng runtime entities có identity/version riêng.

### PA02-C02 — Project Structure & Document Workspace

Cho phép CRUD project tree thực tế:

- folder;
- document;
- move/rename/reorder;
- archive/restore;
- create blank/from template;
- document version/history/diff;
- document section management.

Move/rename không làm thay đổi stable `DocumentId`, human key hoặc semantic object identity.

### PA02-C03 — Semantic Object Management

Cho phép tạo và quản lý các project objects có stable ID/version:

- Goal;
- Flow/Capability;
- Requirement;
- Rule;
- Acceptance Criterion;
- Design Decision/Specification;
- Deliverable;
- Verification Definition;
- các object type được template enable.

Một semantic object có thể được render trong nhiều document thông qua placement nhưng chỉ có một canonical state.

### PA02-C04 — Requirement Decomposition & Drill-down

Requirement không chỉ là một dòng text. Hệ thống phải cho phép drill-down có kiểm soát:

```text
Goal
  ↓
Flow / Capability
  ↓
Requirement
  ├── Business Rules
  ├── Acceptance Criteria
  ├── Design
  ├── Deliverables
  ├── Tasks
  └── Verification
```

System phải trả lời được ít nhất:

- Requirement còn thiếu acceptance criterion không?
- Requirement có design coverage chưa?
- Requirement đã phân rã thành đủ deliverable chưa?
- Deliverable nào chưa có task?
- Task nào chưa hoàn thành?
- Requirement đã được verify trên implementation revision hiện tại chưa?

### PA02-C05 — Traceability Graph

Quản lý generic typed relation giữa các traceable entities.

Phải hỗ trợ:

- create/update/delete relation theo policy;
- inbound/outbound/reverse query;
- backlinks;
- transitive traversal;
- graph depth/type filter;
- provenance;
- version-sensitive relation;
- stale detection.

Folder hierarchy không dùng traceability relation.

### PA02-C06 — Deliverable & Design Coverage

Quản lý software outputs như:

```text
Screen
API
BatchJob
Event
File
Report
Notification
Interface
DataObject
DatabaseObject
Configuration
DeploymentArtifact
```

Deliverable lifecycle:

```text
Planned → Specified → Implemented → Verified → Accepted → Deprecated
```

Template policy quyết định một deliverable type cần những design products nào trước khi được `Specified` hoặc task được `Ready`.

### PA02-C07 — Project Tasklist & Planning

Project tasklist là runtime plan, khác `Tasklist Template` ở PA-01.

Task có:

- stable ID/key;
- objective;
- status;
- assignee;
- priority;
- milestone/phase;
- dependencies;
- read set / required context;
- write scope;
- target deliverables;
- acceptance criteria;
- verification requirements;
- result/evidence.

Task có thể được tạo từ task template hoặc tạo thủ công theo permission/policy.

### PA02-C08 — Verification & Evidence

Tách rõ:

```text
VerificationDefinition
    ↓ executed-as
VerificationRun
    ↓ produces
Evidence
```

Verification phải gắn với requirement/deliverable và exact target version hoặc implementation revision khi cần.

### PA02-C09 — Coverage & Completeness Engine

Coverage không chỉ là task percentage. Engine phải evaluate project graph theo template governance rules.

Ví dụ trạng thái requirement:

```text
Requirement Defined      ✓
Acceptance Coverage      ✓
Design Coverage          80%
Deliverable Coverage     ✓
Task Coverage            ✓
Implementation           60%
Verification             20%
Overall                   In Progress
```

Coverage engine phải trả ra cả **score/state và missing items/reasons**, để human hoặc AI biết chính xác cần làm gì tiếp.

### PA02-C10 — Baseline, Change & Impact

Baseline cố định exact entity versions. Khi semantic object thay đổi, impact engine traverse traceability graph theo relation policy để tạo potential impacts.

Impact disposition tối thiểu:

```text
UpdateRequired
ReviewRequired
RevalidationRequired
ReplanRequired
NoChangeRequired
Obsolete
```

Structural-only changes như move/rename document mặc định không tạo semantic impact.

---

# 4. PA-03 — Work Management & Human-AI Collaboration

## 4.1 Product question

> Ai đang làm gì, tiến độ thực tế ra sao, requirement nào chưa hoàn thành, công việc nào bị block, AI agent và con người phối hợp thế nào?

PA-03 không tạo thêm một project model khác. Nó là **execution/collaboration layer trên PA-02 project graph**.

## 4.2 Capability map

### PA03-C01 — Task Board & Work Queue

Các view tối thiểu:

- list;
- Kanban/status board;
- assignee queue;
- milestone/phase view;
- blocked tasks;
- review queue;
- dependency view.

Task lifecycle:

```text
Draft → Ready → InProgress → Review → Done
          ↕          ↕
        Blocked    Blocked
```

`Cancelled` là terminal state riêng.

### PA03-C02 — Human & AI Assignment

Human, AI Agent và Service Account đều là `Principal` có project membership, permission và scope.

Task assignment phải hỗ trợ:

- human assignee;
- AI agent assignee;
- handoff AI → human;
- handoff human → AI;
- reassignment;
- review ownership;
- machine credential/scope enforcement.

### PA03-C03 — AI Task Context Resolution

Điểm khác biệt cốt lõi của sản phẩm: AI coding agent không cần tự crawl cả repository để đoán context.

Từ một `TaskId`, hệ thống phải resolve được một deterministic context bundle:

```text
Task
├── objective
├── upstream requirements + exact versions
├── acceptance criteria
├── related design specifications
├── target deliverables
├── required documents/sections
├── dependencies
├── allowed write scope
├── verification requirements
├── project baseline
└── relevant implementation artifacts
```

Task context phải machine-readable và export/API accessible.

### PA03-C04 — Execution Result & Evidence

AI/human phải có thể submit:

- TaskResult;
- summary;
- artifacts;
- commit/PR references;
- verification runs;
- evidence;
- blocker reason;
- review request.

`Task Done` không tự động làm `Deliverable Accepted` hoặc `Requirement Complete`.

### PA03-C05 — Review, Blocker & Handoff Management

Phải quản lý được:

- blocked reason;
- missing prerequisite;
- missing/ambiguous requirement;
- missing design;
- failed verification;
- reviewer;
- requested changes;
- resolution history.

Một AI agent gặp thiếu context nên tạo blocker/change request thay vì tự suy đoán requirement.

### PA03-C06 — Project Progress Dashboard

Dashboard phải cho thấy progress từ nhiều góc nhìn, không chỉ task count:

```text
Requirement coverage
Design coverage
Deliverable lifecycle
Task execution
Verification status
Milestone outcome
Blocked work
Stale dependencies
Change/impact backlog
```

### PA03-C07 — Requirement Completion Dashboard

Đây là view cốt lõi để trả lời câu hỏi **AI/human đã hoàn thành đủ requirement chưa**.

Ví dụ:

```text
REQ-001  Complete
REQ-002  Missing API error-handling design
REQ-003  Implementation incomplete: TASK-BE-031
REQ-004  Implementation done, verification stale
REQ-005  Blocked by unresolved business rule
```

Click vào requirement phải drill-down được toàn bộ path requirement → design → deliverable → task → artifact → verification.

### PA03-C08 — Agent / Team Activity & Audit

Cho phép quan sát:

- ai/human đang xử lý task nào;
- recent transitions;
- task result submissions;
- rejected/reopened work;
- permission-sensitive actions;
- agent execution references;
- audit trail.

---

# 5. Platform / Cross-cutting Capabilities

Các capability này hỗ trợ cả ba Product Area.

## PF-01 — Identity, Membership & RBAC

Project-scoped permission cho Human / AI Agent / Service.

## PF-02 — Stable Identity, Versioning & Baseline

Stable technical IDs, human keys, immutable published/baseline versions, optimistic concurrency và stale detection.

## PF-03 — Audit & Provenance

Mọi mutation quan trọng phải biết ai làm, khi nào, từ version nào sang version nào và nguồn nào.

## PF-04 — API-first & Machine Access

Mọi capability chính phải expose application/API use case tương ứng; machine principal có credential và scope riêng.

## PF-05 — Export / Repository Integration / Sync

Export project structure + semantic graph + exact versions thành human-readable và machine-readable projection. Repository không trở thành canonical governance store thứ hai.

## PF-06 — Search & Query

Search theo key/type/text/status/owner và graph/coverage/backlink queries.

---

# 6. Product Area Dependency

```text
PA-01 Template & Methodology
   │
   │ defines structure, schemas, task blueprints, policies
   ▼
PA-02 Project Workspace & Traceability
   │
   │ creates runtime documents, objects, graph, tasklist, coverage
   ▼
PA-03 Work Management & Human-AI Collaboration
   │
   │ executes tasks, submits artifacts/evidence, reviews progress
   └───────────────┐
                   │ updates runtime project state
                   ▼
                 PA-02
```

PA-03 phụ thuộc PA-02. PA-02 có thể hoạt động mà không có AI. PA-01 có thể quản lý/publish template mà chưa instantiate project.

---

# 7. Template vs Runtime Mapping

| Template object | Runtime project object |
|---|---|
| `ProjectTemplateVersion` | `Project` + provenance tới template version |
| `ProjectStructureTemplateNode` | `ProjectStructureNode` |
| `DocumentTemplateVersion` | `Document` + initial `DocumentVersion` |
| semantic object schema | `KnowledgeObject` / typed project object |
| relation type definition | `Relation` instances |
| deliverable policy | `Deliverable` validation/lifecycle rules |
| `TaskTemplate` | `Task` |
| `TasklistTemplate` | project task group / generated tasks + dependencies |
| coverage/governance policy | evaluated coverage/completeness result |
| DoR / DoD policy | task transition validation |

Template object không giữ runtime status/progress của project instance.

---

# 8. Canonical User Journeys

## Journey A — Define methodology

```text
Create Project Template
→ define structure
→ define document templates
→ define semantic/relation schema
→ define deliverable rules
→ define task/tasklist templates
→ define coverage/DoR/DoD rules
→ validate
→ publish template version
```

## Journey B — Start a project

```text
Create Project
→ choose exact template version
→ instantiate structure/documents/policies
→ create/import requirements
→ drill down requirements
→ define design/deliverables
→ instantiate/create tasklist
→ baseline planning scope
```

## Journey C — Human + AI execution

```text
Select Ready Task
→ resolve deterministic context
→ assign human/AI
→ execute
→ submit result/artifacts/evidence
→ review
→ update task/deliverable/verification state
→ recalculate requirement coverage
→ dashboard reflects new progress
```

## Journey D — Requirement change

```text
Change Requirement
→ create new semantic version
→ detect stale consumers
→ traverse potential impact
→ disposition impacts
→ create/update tasks
→ execute/reverify
→ restore coverage
```

---

# 9. MVP sequencing

```text
MVP-1  PA-01 Template foundation
       Project/Structure/Document/Tasklist templates + version/publish

MVP-2  PA-02 Project instantiation & workspace
       Instantiate template + project structure + documents + semantic objects

MVP-3  PA-02 Traceability & coverage
       Relations + requirement drill-down + deliverables + tasklist + coverage engine

MVP-4  PA-03 Work management
       Task board + human/AI assignment + task context + result/review/blocker

MVP-5  PA-03 Dashboards
       Requirement completion + project progress + agent/team activity

MVP-6  Change/impact + external integration
       Baseline/stale/impact + export/API/repository sync
```

---

# 10. Product-level acceptance

Sản phẩm đạt core differentiation khi một user có thể:

1. định nghĩa và publish một project methodology dưới dạng template có document structure, semantic rules và tasklist templates;
2. tạo một project thật từ exact template version;
3. CRUD runtime documents/tasks mà không làm mất stable identity;
4. drill-down một requirement và biết chính xác còn thiếu design/deliverable/task/verification nào;
5. giao task cho human hoặc AI agent với deterministic read context + write scope + acceptance/verification contract;
6. nhận kết quả/evidence và cập nhật coverage dựa trên graph thay vì dựa vào lời khai của agent;
7. nhìn dashboard và thấy progress theo requirement/deliverable/verification, không chỉ theo số task Done;
8. khi requirement thay đổi, xác định được các downstream items có potential impact và các task cần replan/reverify.
