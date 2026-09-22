# Software Project Governance SaaS

Repository này thiết kế một **SaaS quản lý software project theo hướng traceability-first**. Điểm khác biệt cốt lõi không phải là lưu Markdown tốt hơn, mà là biến methodology, requirement, design, deliverable, task, verification và quan hệ giữa chúng thành dữ liệu có cấu trúc để cả human lẫn AI agent có thể làm việc trên cùng một project model.

`docs/sample-project/` chỉ là **reference/acceptance fixture**. Nó minh họa một bộ tài liệu mà app phải có thể biểu diễn, tạo từ template, chỉnh sửa và export; nó không phải product requirement source.

## 1. Product decomposition

Canonical product decomposition nằm tại [PRODUCT-AREAS.md](./PRODUCT-AREAS.md).

Sản phẩm được chia thành ba Product Area:

```text
PA-01 Template & Methodology Management
        ↓ instantiate
PA-02 Project Workspace & Traceability
        ↓ execute / collaborate
PA-03 Work Management & Human-AI Collaboration
```

Các capability dùng chung như Identity/RBAC, stable identity/versioning, audit, API, export/sync và search được xem là Platform / Cross-cutting capabilities.

---

# 2. PA-01 — Template & Methodology Management

PA-01 trả lời câu hỏi:

> Một software project chuẩn phải được tổ chức, breakdown, giao việc và kiểm chứng như thế nào?

Template layer quản lý:

```text
Project Template
├── Structure Template
│   ├── Folder Template Node
│   └── Document Template Node
├── Document Templates
├── Semantic Object Schemas
├── Relation Type Definitions
├── Deliverable Policies
├── Task Templates
├── Tasklist Templates
├── Coverage / Completeness Rules
├── Definition of Ready / Done
└── Role / Export / Lifecycle Policies
```

Template có version và lifecycle riêng. Published template version immutable. Project instance phải giữ provenance tới exact template version đã dùng.

**Tasklist cũng phải được template hóa.** Ví dụ một `API Deliverable` có thể yêu cầu task blueprint gồm Backend Implementation → Unit Test → Integration Test → Review, kèm dependency, required context, target deliverable, verification và DoR/DoD.

---

# 3. PA-02 — Project Workspace & Traceability

PA-02 trả lời câu hỏi:

> Trong Project X hiện có những requirement, design, deliverable, document và task nào; chúng liên quan với nhau thế nào; requirement đã drill-down tới đâu và còn thiếu gì?

Khi user tạo project từ một `ProjectTemplateVersion`, app instantiate runtime project state:

```text
Template                           Project Runtime
--------                           ---------------
Structure Template        →        Project Structure Tree
Document Template         →        Document + initial version
Semantic Schema           →        Project semantic objects
Relation Definition       →        Relation instances
Task / Tasklist Template  →        Runtime Tasks + Dependencies
Governance Policy         →        Coverage / transition validation
```

Core project model gồm hai cấu trúc độc lập nhưng liên kết:

```text
A. Project Structure Tree

Project
└── Folder / Document nodes

→ navigation, ordering, CRUD, export path

B. Traceability Graph

Goal / Flow / Requirement / Rule / Design / Document
                  ↕ typed relations
Deliverable / Task / Verification / Artifact / Milestone / Change

→ dependency, coverage, backlinks, impact, task context
```

Folder/document parent-child không phải semantic relation. Move/rename document không làm thay đổi `DocumentId`, human key hoặc semantic object identity.

## 3.1 Requirement drill-down

Requirement phải drill-down được theo graph, không chỉ là text:

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

System phải trả lời được:

- requirement còn thiếu acceptance criteria không;
- requirement có đủ design coverage chưa;
- requirement đã sinh đủ deliverable chưa;
- deliverable nào chưa có task;
- task nào chưa hoàn thành;
- implementation đã được verify trên đúng version/revision chưa;
- dependency/input nào đã stale.

## 3.2 Canonical traceability chain

```text
Goal
  ↓ decomposes-to
Business Flow / Capability
  ↓ decomposes-to
Requirement
  ├── governed-by → Business Rule / Policy
  ├── accepted-by → Acceptance Criterion
  ├── satisfied-by → Design Decision / Design Specification
  └── requires → Deliverable

Design Specification
  └── specifies → Deliverable

Task
  └── implements → Deliverable

Verification Definition
  └── verifies → Requirement / Deliverable

Task / TaskResult
  └── produces → Implementation Artifact
```

Reverse views như `required-by`, `specified-by`, `implemented-by`, `verified-by` được generated từ canonical edge, không lưu editable duplicate.

---

# 4. PA-03 — Work Management & Human-AI Collaboration

PA-03 trả lời câu hỏi:

> Ai đang làm gì, tiến độ thực tế ra sao, requirement nào chưa hoàn thành, công việc nào bị block, AI agent và human phối hợp thế nào?

PA-03 là execution/collaboration layer trên runtime project graph của PA-02, không tạo một project model thứ hai.

## 4.1 Task management

Task lifecycle:

```text
Draft → Ready → InProgress → Review → Done
          ↕          ↕
        Blocked    Blocked
```

`Cancelled` là terminal state riêng. `Task Done` không đồng nghĩa `Deliverable Accepted` hay `Requirement Complete`.

Task runtime phải quản lý:

- objective;
- assignee;
- priority;
- milestone/phase;
- dependencies;
- exact read set / required context;
- write scope / target deliverables;
- acceptance criteria;
- verification requirements;
- result/evidence;
- blocker/review state.

## 4.2 Human + AI

Actor trong project được biểu diễn chung:

```text
Principal
├── Human
├── AIAgent
└── Service
        ↓
ProjectMembership
        ↓
Role / Permission / Scope
```

AI không có `AITask` riêng. AI nhận cùng Task model nhưng hệ thống phải resolve được deterministic task context để agent không cần crawl project rồi tự suy đoán.

```text
Task
├── objective
├── upstream requirements + exact versions
├── acceptance criteria
├── design specifications
├── target deliverables
├── required documents/sections
├── dependencies
├── allowed write scope
├── verification requirements
├── baseline
└── relevant implementation artifacts
```

## 4.3 Dashboard

Dashboard phải đo progress theo project graph, không chỉ task count:

```text
Requirement coverage
Design coverage
Deliverable lifecycle
Task execution
Verification status
Milestone outcomes
Blocked work
Stale dependencies
Change / impact backlog
```

Một Requirement Completion view phải có thể cho biết trực tiếp:

```text
REQ-001  Complete
REQ-002  Missing API error-handling design
REQ-003  Implementation incomplete: TASK-BE-031
REQ-004  Implementation done, verification stale
REQ-005  Blocked by unresolved business rule
```

Click vào requirement phải drill-down được toàn bộ path requirement → design → deliverable → task → artifact → verification.

---

# 5. Template vs Runtime boundary

Đây là boundary quan trọng nhất của product model:

| Template | Runtime project |
|---|---|
| `ProjectTemplateVersion` | `Project` |
| `ProjectStructureTemplateNode` | `ProjectStructureNode` |
| `DocumentTemplateVersion` | `Document` + `DocumentVersion` |
| semantic object schema | project semantic objects |
| relation type definition | `Relation` |
| `TaskTemplate` | `Task` |
| `TasklistTemplate` | runtime tasks + dependencies |
| governance/coverage policy | evaluated coverage/completeness |
| DoR/DoD | task transition validation |

Template object không giữ runtime progress của project instance.

---

# 6. Version, baseline, change and impact

Stable identity:

```text
Technical ID  → immutable UUID/ULID
Human Key     → stable project-scoped key
```

Published template version và project baseline version là immutable.

Khi một baseline/versioned semantic entity thay đổi, hệ thống traverse Traceability Graph theo relation policy và tạo **potential impact**, sau đó owner disposition:

```text
UpdateRequired
ReviewRequired
RevalidationRequired
ReplanRequired
NoChangeRequired
Obsolete
```

Structural-only changes như move/rename/reorder document mặc định không tạo semantic impact.

---

# 7. Canonical documents

- [PRODUCT-AREAS.md](./PRODUCT-AREAS.md): canonical product decomposition và capability map.
- [APP-REQUIREMENTS.md](./APP-REQUIREMENTS.md): detailed product requirements organized theo Product Area.
- [APP-DOMAIN-MODEL.md](./APP-DOMAIN-MODEL.md): canonical domain concepts, entities và invariants.
- [docs/DOCUMENT-ENTITY-MODEL.md](./docs/DOCUMENT-ENTITY-MODEL.md): document/structure/placement model và boundary với traceability graph.
- [DESIGN-DELIVERABLE-TASK-GOVERNANCE.md](./DESIGN-DELIVERABLE-TASK-GOVERNANCE.md): governance từ requirement → design → deliverable → task → verification.
- [INTEGRATION-SYNC-CHANGE-MANAGEMENT.md](./INTEGRATION-SYNC-CHANGE-MANAGEMENT.md): integration, export/sync, baseline, stale detection và impact protocol.
- [`docs/sample-project/`](./docs/sample-project/): non-canonical acceptance fixture.

Thứ tự ưu tiên khi có mâu thuẫn:

```text
PRODUCT-AREAS.md
    ↓
APP-REQUIREMENTS.md
    ↓
APP-DOMAIN-MODEL.md
    ↓
specialized design/governance docs
```

`docs/sample-project/` không override product definition.

---

# 8. MVP sequence

```text
MVP-1  PA-01 Template foundation
       Project/Structure/Document/Tasklist templates + version/publish

MVP-2  PA-02 Project instantiation & workspace
       Runtime structure + documents + semantic objects

MVP-3  PA-02 Traceability & coverage
       Requirement drill-down + relations + deliverables + runtime tasklist

MVP-4  PA-03 Work management
       Task board + human/AI assignment + deterministic task context + review/blocker

MVP-5  PA-03 Dashboards
       Requirement completion + project progress + agent/team activity

MVP-6  Change/impact + integration
       Baseline/stale/impact + API/export/repository sync
```

---

# 9. Technical direction

MVP:

```text
React + TypeScript
        ↓
ASP.NET Core API
        ↓
Modular Monolith / Application + Domain
        ↓
PostgreSQL
```

PostgreSQL lưu canonical entity state và relation edges. Recursive CTE/index đủ cho MVP; chỉ cân nhắc graph database khi có evidence rõ ràng về scale/query pattern mà relational model không đáp ứng được.

> **Core differentiation của sản phẩm là: methodology được template hóa, project runtime có traceability graph, requirement có thể đo mức drill-down/completion, và human + AI cùng thực thi task trên deterministic context thay vì tự đoán từ một đống Markdown.**