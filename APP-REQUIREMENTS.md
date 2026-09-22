# Software Project Governance SaaS — Product Requirements

## 1. Purpose and product boundary

Sản phẩm là một **SaaS quản lý software project theo hướng traceability-first**. Mục tiêu không phải chỉ lưu Markdown hoặc thay thế Jira/GitHub, mà là quản lý methodology, project knowledge, requirement, design, deliverable, task, verification và quan hệ giữa chúng dưới dạng structured data để human và AI agent có thể cùng lập kế hoạch, thực thi và kiểm chứng công việc.

Canonical product decomposition được định nghĩa tại [PRODUCT-AREAS.md](./PRODUCT-AREAS.md):

```text
PA-01 Template & Methodology Management
        ↓ instantiate
PA-02 Project Workspace & Traceability
        ↓ execute / collaborate
PA-03 Work Management & Human-AI Collaboration
```

`docs/sample-project/` chỉ là reference/acceptance fixture. Nội dung sample không phải requirement source của sản phẩm.

SaaS là source of truth cho governance/semantic project state. Source repository là source of truth cho source code. CI provider là source of truth cho CI execution result.

---

## 2. Requirement identification

Requirement ID dùng format:

```text
PAxx-Cyy-FR-zzz   Functional Requirement
PAxx-Cyy-BR-zzz   Business/Governance Rule
PFxx-FR-zzz       Platform/Cross-cutting Requirement
AC-PAxx-zzz       Product Area Acceptance Criterion
```

Ví dụ:

```text
PA01-C03-FR-001   Create Document Template
PA02-C04-FR-004   Evaluate Requirement Drill-down
PA03-C03-FR-002   Resolve AI Task Context
```

Mỗi design decision, screen, API, task phát triển sản phẩm và test sau này phải reference các requirement IDs tương ứng.

---

# 3. PA-01 — Template & Methodology Management

## 3.1 Product objective

PA-01 định nghĩa **cách một software project nên được tổ chức và quản trị** trước khi có project instance cụ thể.

Template layer là metamodel/methodology, không lưu runtime task progress của project thật.

---

## 3.2 PA01-C01 — Project Template Management

### Functional requirements

**PA01-C01-FR-001 — Create Project Template**  
User có permission phải tạo được `ProjectTemplate` với key, name, description và ownership metadata.

**PA01-C01-FR-002 — Version Project Template**  
Mọi thay đổi publishable phải nằm trong `ProjectTemplateVersion`. Project phải reference exact version được dùng để instantiate.

**PA01-C01-FR-003 — Publish Template Version**  
System phải hỗ trợ lifecycle tối thiểu:

```text
Draft → Published → Deprecated
```

**PA01-C01-FR-004 — Clone/Fork Template**  
User có thể tạo template mới từ template/version hiện có mà không làm thay đổi source template.

**PA01-C01-BR-001 — Published Version Immutable**  
Published template version không được silently edit. Thay đổi phải tạo version mới.

---

## 3.3 PA01-C02 — Structure Template Management

Template phải định nghĩa explicit folder/document tree:

```text
ProjectStructureTemplateNode
├── Folder
└── Document → DocumentTemplateVersion
```

**PA01-C02-FR-001** Create root/sub-folder template node.  
**PA01-C02-FR-002** Create document template node.  
**PA01-C02-FR-003** Rename/move/reorder node.  
**PA01-C02-FR-004** Mark node required/optional.  
**PA01-C02-FR-005** Archive/remove draft node theo policy.  
**PA01-C02-FR-006** Preview full structure tree trước khi publish.

**PA01-C02-BR-001** Structure tree không được cycle.  
**PA01-C02-BR-002** Document node phải reference valid `DocumentTemplateVersion`.  
**PA01-C02-BR-003** Folder node không phải Document Template.

---

## 3.4 PA01-C03 — Document Template Management

Document template phải support các loại như Requirement Specification, Architecture, API Specification, Screen Specification, Batch/Job Specification, Data Design, Test Strategy và Runbook.

**PA01-C03-FR-001** Create/edit/version Document Template.  
**PA01-C03-FR-002** Define section tree.  
**PA01-C03-FR-003** Mark section required/optional.  
**PA01-C03-FR-004** Define initial text/content placeholder.  
**PA01-C03-FR-005** Define allowed semantic object types per section.  
**PA01-C03-FR-006** Define document-level validation rules.  
**PA01-C03-FR-007** Preview rendered template.

**PA01-C03-BR-001** DocumentTemplateVersion là template artifact; runtime Document có identity/version riêng.

---

## 3.5 PA01-C04 — Semantic Schema & Relation Template

Template phải định nghĩa software-project object types được enable và relation vocabulary.

Initial semantic types tối thiểu:

```text
Goal
BusinessCapability
BusinessFlow
Requirement
BusinessRule
AcceptanceCriterion
DesignDecision
DesignSpecification
ArchitectureRule
DataSpecification
InterfaceContract
Policy
Standard
```

Initial traceable runtime types còn gồm Document, Deliverable, Task, VerificationDefinition, Milestone, ImplementationArtifact và ChangeRequest.

**PA01-C04-FR-001** Configure enabled object types.  
**PA01-C04-FR-002** Configure object validation/schema metadata.  
**PA01-C04-FR-003** Configure relation type definition.  
**PA01-C04-FR-004** Configure allowed source/target types.  
**PA01-C04-FR-005** Configure canonical direction và reverse display label.  
**PA01-C04-FR-006** Configure acyclic/version-sensitive/cardinality policy.  
**PA01-C04-FR-007** Configure impact propagation mode/direction.

Initial canonical relation vocabulary:

```text
decomposes-to
governed-by
accepted-by
satisfied-by
requires
specifies
implements
produces
verifies
depends-on
references
supersedes
impacts
```

**PA01-C04-BR-001** Reverse relation là generated view, không lưu editable duplicate edge.  
**PA01-C04-BR-002** `contained-in` không dùng cho folder/document hierarchy.

---

## 3.6 PA01-C05 — Deliverable Policy Template

Template phải mô tả project output types và design/verification rule tương ứng.

Initial deliverable types:

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
DocumentationDeliverable
```

**PA01-C05-FR-001** Configure enabled deliverable types.  
**PA01-C05-FR-002** Configure required design products per deliverable type.  
**PA01-C05-FR-003** Configure required verification types.  
**PA01-C05-FR-004** Configure lifecycle transition guards.

Example:

```text
API Deliverable requires before Specified:
- API Contract
- Authorization Design
- Validation Design
- Error Handling Design
- Observability Design
```

---

## 3.7 PA01-C06 — Task Template & Tasklist Template

Task phải được template hóa ở methodology level.

**PA01-C06-FR-001 — Task Template**  
Template phải định nghĩa được objective pattern, required context, target type, write scope, acceptance policy, verification requirements, default role và DoR/DoD.

**PA01-C06-FR-002 — Tasklist Template**  
Template phải định nghĩa được một tập task blueprints cùng dependencies.

Example:

```text
API Implementation Tasklist
├── T1 Backend implementation
├── T2 Unit test              depends-on T1
├── T3 Integration test       depends-on T1
└── T4 Review                 depends-on T2,T3
```

**PA01-C06-FR-003** Task template có thể bind vào deliverable type hoặc work-package type.  
**PA01-C06-FR-004** Tasklist template phải preview được dependency graph.  
**PA01-C06-FR-005** Validate dependency cycle trước publish.  
**PA01-C06-FR-006** Configure required context selectors thay vì hard-code document paths.

**PA01-C06-BR-001** Runtime status không lưu trên TaskTemplate/TasklistTemplate.  
**PA01-C06-BR-002** Instantiate template phải sinh Task/TaskDependency runtime có identity riêng.

---

## 3.8 PA01-C07 — Governance, Coverage, DoR & DoD Policy

Template phải định nghĩa được khi nào project object được coi là đủ sâu/đủ điều kiện.

**PA01-C07-FR-001** Configure Requirement completeness rules.  
**PA01-C07-FR-002** Configure Deliverable readiness/completeness rules.  
**PA01-C07-FR-003** Configure Task Definition of Ready.  
**PA01-C07-FR-004** Configure Task Definition of Done.  
**PA01-C07-FR-005** Configure baseline/approval requirements.  
**PA01-C07-FR-006** Configure mandatory relation/coverage rules.

Example:

```text
Requirement DesignComplete when:
- at least one AcceptanceCriterion exists
- required DesignSpecification types exist
- required Deliverables are linked

Task Ready when:
- required context resolved
- dependency states satisfied
- target deliverable declared
- mandatory design baseline available
- acceptance criteria available
```

---

## 3.9 PA01-C08 — Template Validation

**PA01-C08-FR-001** Validate structure tree.  
**PA01-C08-FR-002** Validate referenced template versions.  
**PA01-C08-FR-003** Validate schema/relation definitions.  
**PA01-C08-FR-004** Validate task dependency graph.  
**PA01-C08-FR-005** Validate task input/output selectors.  
**PA01-C08-FR-006** Validate governance rules reference existing types.  
**PA01-C08-FR-007** Prevent publish nếu blocking validation errors còn tồn tại.

---

# 4. PA-02 — Project Workspace & Traceability

## 4.1 Product objective

PA-02 quản lý **runtime project state** được instantiate từ template và thay đổi trong suốt project lifecycle.

Core mental model phải tách:

```text
Project Structure Tree                 Traceability Graph
----------------------                 ------------------
Folder / Document hierarchy            Typed semantic/dependency relations
navigation / ordering / path           coverage / backlinks / impact / context
```

---

## 4.2 PA02-C01 — Project Instantiation

**PA02-C01-FR-001** Create Project từ exact `ProjectTemplateVersion`.  
**PA02-C01-FR-002** Instantiate structure nodes.  
**PA02-C01-FR-003** Instantiate documents/initial versions.  
**PA02-C01-FR-004** Activate template semantic/relation policies cho project.  
**PA02-C01-FR-005** Instantiate configured initial task/tasklist blueprints.  
**PA02-C01-FR-006** Record provenance tới exact source template/version.

**PA02-C01-BR-001** Runtime entities có identity/version riêng với template entities.  
**PA02-C01-BR-002** Template update không silently rewrite existing project runtime state.

---

## 4.3 PA02-C02 — Project Structure & Document Workspace

MVP node types:

```text
Folder
Document
```

**PA02-C02-FR-001** Create folder/document node.  
**PA02-C02-FR-002** Create document blank/from template.  
**PA02-C02-FR-003** Rename/move/reorder.  
**PA02-C02-FR-004** Archive/restore.  
**PA02-C02-FR-005** Query tree/subtree/canonical path.  
**PA02-C02-FR-006** Edit document content/sections.  
**PA02-C02-FR-007** Save/view/compare document versions.  
**PA02-C02-FR-008** Search document by key/type/status/owner/text.

**PA02-C02-BR-001** Folder không phải Document.  
**PA02-C02-BR-002** Move/rename không làm đổi `DocumentId`, human key hoặc embedded semantic identity.  
**PA02-C02-BR-003** Baseline/referenced object không hard-delete theo cách làm mất lịch sử.

---

## 4.4 PA02-C03 — Semantic Object Management

**PA02-C03-FR-001** Create/update/version semantic object.  
**PA02-C03-FR-002** Assign stable project-scoped human key.  
**PA02-C03-FR-003** Place/render object trong one-or-more documents.  
**PA02-C03-FR-004** View all placements.  
**PA02-C03-FR-005** Separate semantic version khỏi document version.

**PA02-C03-BR-001** `KnowledgePlacement` trả lời object được render ở đâu; nó không thay semantic Relation.

---

## 4.5 PA02-C04 — Requirement Decomposition & Drill-down

Requirement phải được quản lý như traceable semantic object, không chỉ text paragraph.

**PA02-C04-FR-001** Link Goal/Flow/Capability tới Requirement.  
**PA02-C04-FR-002** Link Requirement tới Business Rules/Policies.  
**PA02-C04-FR-003** Link Requirement tới Acceptance Criteria.  
**PA02-C04-FR-004** Link Requirement tới Design Decisions/Specifications.  
**PA02-C04-FR-005** Link Requirement tới required Deliverables.  
**PA02-C04-FR-006** Trace Requirement tới Tasks thông qua deliverable/task graph.  
**PA02-C04-FR-007** Trace Requirement tới Verification/Evidence.  
**PA02-C04-FR-008** Drill-down từ parent requirement tới lower-level requirements khi decomposition policy cho phép.

System phải trả lời được:

```text
Requirement đã định nghĩa đủ chưa?
Acceptance criteria đủ chưa?
Design coverage đủ chưa?
Deliverable coverage đủ chưa?
Task coverage đủ chưa?
Implementation hoàn thành chưa?
Verification current chưa?
```

---

## 4.6 PA02-C05 — Traceability Relation Management

**PA02-C05-FR-001** Create relation.  
**PA02-C05-FR-002** Validate source/target/type theo project policy.  
**PA02-C05-FR-003** Update allowed metadata.  
**PA02-C05-FR-004** Delete/archive theo lifecycle/audit policy.  
**PA02-C05-FR-005** Query inbound/outbound.  
**PA02-C05-FR-006** Generate reverse/backlink view.  
**PA02-C05-FR-007** Traverse transitive graph với depth/type filter.  
**PA02-C05-FR-008** Inspect provenance.  
**PA02-C05-FR-009** Detect stale version-sensitive relation/input.

Canonical direction examples:

```text
Requirement --requires--> Deliverable
Requirement --satisfied-by--> DesignSpecification
DesignSpecification --specifies--> Deliverable
Task --implements--> Deliverable
VerificationDefinition --verifies--> Requirement / Deliverable
Task/TaskResult --produces--> ImplementationArtifact
```

---

## 4.7 PA02-C06 — Deliverable & Design Coverage

Deliverable lifecycle:

```text
Planned → Specified → Implemented → Verified → Accepted → Deprecated
```

**PA02-C06-FR-001** CRUD Deliverable.  
**PA02-C06-FR-002** Manage Deliverable version/lifecycle.  
**PA02-C06-FR-003** Evaluate mandatory design products theo template policy.  
**PA02-C06-FR-004** Show missing specifications.  
**PA02-C06-FR-005** Link deliverable tới upstream requirements.  
**PA02-C06-FR-006** Link design specifications, tasks và verification qua relation graph.

**PA02-C06-BR-001** `Specified` chỉ đạt khi required design rules pass.  
**PA02-C06-BR-002** `Verified` cần valid VerificationRun cho target version/revision.

---

## 4.8 PA02-C07 — Runtime Tasklist & Planning

Task runtime khác task template.

**PA02-C07-FR-001** Create task manually hoặc from template.  
**PA02-C07-FR-002** Instantiate tasklist template.  
**PA02-C07-FR-003** Manage priority, assignee, milestone/phase.  
**PA02-C07-FR-004** Manage task dependencies.  
**PA02-C07-FR-005** Bind exact required inputs/read set.  
**PA02-C07-FR-006** Bind allowed write scope/target deliverables.  
**PA02-C07-FR-007** Bind acceptance criteria/verification requirements.  
**PA02-C07-FR-008** Replan tasks khi impact/change yêu cầu.

Task lifecycle:

```text
Draft → Ready → InProgress → Review → Done
          ↕          ↕
        Blocked    Blocked
```

`Cancelled` là terminal state riêng.

**PA02-C07-BR-001** Task Ready phải pass configured DoR.  
**PA02-C07-BR-002** Task Done không đồng nghĩa Deliverable Accepted.

---

## 4.9 PA02-C08 — Verification & Evidence

```text
VerificationDefinition
   ↓ executed-as
VerificationRun
   ↓ produces
Evidence
```

**PA02-C08-FR-001** CRUD VerificationDefinition.  
**PA02-C08-FR-002** Link definition tới Requirement/Deliverable.  
**PA02-C08-FR-003** Record VerificationRun.  
**PA02-C08-FR-004** Pin target entity/version/revision.  
**PA02-C08-FR-005** Attach Evidence.  
**PA02-C08-FR-006** Determine current/stale verification state.

---

## 4.10 PA02-C09 — Coverage & Completeness Engine

Coverage engine là core differentiation của product.

**PA02-C09-FR-001** Evaluate requirement completeness từ governance policy.  
**PA02-C09-FR-002** Evaluate design coverage.  
**PA02-C09-FR-003** Evaluate deliverable coverage.  
**PA02-C09-FR-004** Evaluate task planning/execution coverage.  
**PA02-C09-FR-005** Evaluate verification coverage/currentness.  
**PA02-C09-FR-006** Return missing items/reasons, không chỉ percentage.  
**PA02-C09-FR-007** Query orphan objects và tasks không có upstream reason.  
**PA02-C09-FR-008** Recalculate affected coverage khi graph/version/status thay đổi.

Example result:

```text
REQ-003
Requirement Defined      Complete
Acceptance Coverage      Complete
Design Coverage          Missing ErrorHandlingSpecification
Deliverable Coverage     Complete
Task Coverage            Complete
Implementation           2/3 Tasks Done
Verification             NotCurrent
Overall                  InProgress
```

---

## 4.11 PA02-C10 — Baseline, Change & Impact

**PA02-C10-FR-001** Create immutable ProjectBaseline chứa exact entity versions.  
**PA02-C10-FR-002** Create semantic version mới thay vì overwrite baseline content.  
**PA02-C10-FR-003** Detect stale consumers.  
**PA02-C10-FR-004** Traverse potential impact graph theo relation policy.  
**PA02-C10-FR-005** Record ImpactItem.  
**PA02-C10-FR-006** Disposition impact.  
**PA02-C10-FR-007** Link impact tới replanning/reverification tasks.

Impact dispositions:

```text
UpdateRequired
ReviewRequired
RevalidationRequired
ReplanRequired
NoChangeRequired
Obsolete
```

**PA02-C10-BR-001** Structural-only move/rename/reorder mặc định không propagate semantic impact.

---

# 5. PA-03 — Work Management & Human-AI Collaboration

## 5.1 Product objective

PA-03 cung cấp execution/collaboration experience trên project graph của PA-02 để human và AI agent cùng làm việc, review và quan sát progress.

---

## 5.2 PA03-C01 — Task Board & Work Queue

**PA03-C01-FR-001** List view.  
**PA03-C01-FR-002** Kanban/status board.  
**PA03-C01-FR-003** Assignee queue.  
**PA03-C01-FR-004** Milestone/phase view.  
**PA03-C01-FR-005** Blocked task view.  
**PA03-C01-FR-006** Review queue.  
**PA03-C01-FR-007** Dependency view/filter.

---

## 5.3 PA03-C02 — Human & AI Assignment

Actor model:

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

**PA03-C02-FR-001** Assign task cho human.  
**PA03-C02-FR-002** Assign task cho AI agent.  
**PA03-C02-FR-003** Reassign task.  
**PA03-C02-FR-004** Human → AI handoff.  
**PA03-C02-FR-005** AI → Human handoff/review.  
**PA03-C02-FR-006** Enforce membership/permission/scope cho cả human và machine actor.

**PA03-C02-BR-001** AI không có một task/domain model riêng.

---

## 5.4 PA03-C03 — Deterministic Task Context Resolution

Từ `TaskId`, system phải resolve context machine-readably thay vì để AI tự crawl toàn project.

**PA03-C03-FR-001** Resolve task metadata/version.  
**PA03-C03-FR-002** Resolve upstream Requirement/Rule/AcceptanceCriterion exact versions.  
**PA03-C03-FR-003** Resolve relevant Design Specifications.  
**PA03-C03-FR-004** Resolve target Deliverables.  
**PA03-C03-FR-005** Resolve required Documents/sections.  
**PA03-C03-FR-006** Resolve dependencies.  
**PA03-C03-FR-007** Resolve allowed write scope.  
**PA03-C03-FR-008** Resolve verification requirements.  
**PA03-C03-FR-009** Resolve current ProjectBaseline.  
**PA03-C03-FR-010** Resolve relevant ImplementationArtifacts.

Context response phải expose exact IDs/version refs và reason/path vì sao item được đưa vào context.

---

## 5.5 PA03-C04 — Execution Result & Evidence

**PA03-C04-FR-001** Human/AI submit TaskResult.  
**PA03-C04-FR-002** Attach implementation artifacts.  
**PA03-C04-FR-003** Attach commit/PR/external execution reference.  
**PA03-C04-FR-004** Submit verification run/evidence.  
**PA03-C04-FR-005** Submit blocker reason.  
**PA03-C04-FR-006** Submit for review.  
**PA03-C04-FR-007** Reopen/reject result với rationale.

Task result phải auditable theo exact task version và submitter.

---

## 5.6 PA03-C05 — Review, Blocker & Handoff

**PA03-C05-FR-001** Record blocked reason/category.  
**PA03-C05-FR-002** Link blocker tới missing prerequisite/requirement/design/dependency.  
**PA03-C05-FR-003** Assign reviewer.  
**PA03-C05-FR-004** Record requested changes.  
**PA03-C05-FR-005** Record resolution history.  
**PA03-C05-FR-006** Create change/request-for-clarification khi requirement/design ambiguous.

**PA03-C05-BR-001** AI gặp thiếu context không được coi suy đoán của nó là canonical requirement change; phải surface blocker/proposal.

---

## 5.7 PA03-C06 — Project Progress Dashboard

Dashboard tối thiểu phải aggregate:

- Requirement coverage;
- Design coverage;
- Deliverable lifecycle;
- Task execution;
- Verification status;
- Milestone outcomes;
- Blocked/review queues;
- stale dependencies;
- change/impact backlog.

**PA03-C06-FR-001** Project overview summary.  
**PA03-C06-FR-002** Filter theo area/type/owner/milestone/status.  
**PA03-C06-FR-003** Drill-down từ metric về underlying entities.  
**PA03-C06-FR-004** Show reason khi metric incomplete.  
**PA03-C06-FR-005** Distinguish planning progress, implementation progress và verification progress.

---

## 5.8 PA03-C07 — Requirement Completion Dashboard

Đây là key product view.

**PA03-C07-FR-001** List requirements với completion state.  
**PA03-C07-FR-002** Show missing design/deliverable/task/verification reasons.  
**PA03-C07-FR-003** Drill-down Requirement → Design → Deliverable → Task → Artifact → Verification.  
**PA03-C07-FR-004** Show stale verification/dependency separately from never-completed work.  
**PA03-C07-FR-005** Filter requirements theo owner/flow/milestone/completion problem.

Example:

```text
REQ-001  Complete
REQ-002  Missing design: Error Handling
REQ-003  Implementation incomplete: TASK-BE-031
REQ-004  Verification stale after requirement v4
REQ-005  Blocked by unresolved BR-018
```

---

## 5.9 PA03-C08 — Team / Agent Activity

**PA03-C08-FR-001** Show currently assigned/in-progress work.  
**PA03-C08-FR-002** Show recent task transitions/results.  
**PA03-C08-FR-003** Show AI execution references.  
**PA03-C08-FR-004** Show review/rejection/reopen activity.  
**PA03-C08-FR-005** Show audit trail cho sensitive actions.

---

# 6. Platform / Cross-cutting Requirements

## 6.1 PF-01 — Identity, Membership & RBAC

**PF01-FR-001** User là global human account.  
**PF01-FR-002** Principal types: Human | AIAgent | Service.  
**PF01-FR-003** ProjectMembership là project authorization boundary.  
**PF01-FR-004** Task assignee reference membership, không reference raw User.  
**PF01-FR-005** Backend enforce role/permission/scope.

---

## 6.2 PF-02 — Stable Identity, Versioning & Concurrency

Traceable entities dùng:

```text
Technical ID → immutable UUID/ULID
Human Key    → stable project-scoped key khi cần human reference
```

**PF02-FR-001** Path/title/name không phải identity.  
**PF02-FR-002** Published/baseline versions immutable.  
**PF02-FR-003** Mutation hỗ trợ optimistic concurrency.  
**PF02-FR-004** Version-sensitive consumer phải detect stale state.

---

## 6.3 PF-03 — Audit & Provenance

Audit tối thiểu cho:

- permission/membership change;
- publish/baseline;
- lifecycle transition;
- structure mutation;
- relation mutation;
- task assignment/result/review;
- machine credential lifecycle;
- change/impact disposition;
- sync/import application.

---

## 6.4 PF-04 — API-first & Machine Access

Mọi core capability phải có application/API use case tương ứng.

Tối thiểu:

```text
Project Templates / Versions
Structure & Document Templates
Task / Tasklist Templates
Projects / Project Structure
Documents / Semantic Objects
Relations / Graph / Coverage
Deliverables
Tasks / Context / Transitions / Results
Verification
Baseline / Change / Impact
Dashboard queries
Export / Sync
```

Lifecycle-sensitive mutation dùng command semantics thay vì raw status patch.

Machine principal phải có credential riêng, project scopes, expiry/revocation và audit.

---

## 6.5 PF-05 — Export, Repository Integration & Sync

Export phải giữ:

- project identity;
- exact entity IDs/keys;
- exact versions;
- structure paths;
- relation set;
- baseline/snapshot ID;
- schema version;
- checksums khi cần.

Two-way sync không silently overwrite canonical state; external edits phải đi qua identity/version comparison, conflict detection và reviewed proposal/change flow.

---

## 6.6 PF-06 — Search & Query

**PF06-FR-001** Search theo text/key/type/status/owner.  
**PF06-FR-002** Query structure subtree.  
**PF06-FR-003** Query backlinks/reverse relations.  
**PF06-FR-004** Query graph traversal.  
**PF06-FR-005** Query coverage/completeness.  
**PF06-FR-006** Query impact/staleness.

---

# 7. Screen map by Product Area

## PA-01

```text
SCR-PA01-001 Template Library
SCR-PA01-002 Project Template Editor
SCR-PA01-003 Structure Template Editor
SCR-PA01-004 Document Template Editor
SCR-PA01-005 Schema & Relation Policy Editor
SCR-PA01-006 Deliverable Policy Editor
SCR-PA01-007 Task / Tasklist Template Editor
SCR-PA01-008 Governance & Coverage Rule Editor
SCR-PA01-009 Template Validation / Publish
```

## PA-02

```text
SCR-PA02-001 Create Project
SCR-PA02-002 Project Workspace
SCR-PA02-003 Structure & Document Explorer
SCR-PA02-004 Semantic Object Detail
SCR-PA02-005 Requirement Drill-down
SCR-PA02-006 Traceability Explorer
SCR-PA02-007 Deliverable Inventory / Detail
SCR-PA02-008 Project Tasklist / Planning
SCR-PA02-009 Verification Center
SCR-PA02-010 Change & Impact Center
```

## PA-03

```text
SCR-PA03-001 Task Board
SCR-PA03-002 Task Detail / Context
SCR-PA03-003 Review & Blocker Queue
SCR-PA03-004 Project Progress Dashboard
SCR-PA03-005 Requirement Completion Dashboard
SCR-PA03-006 Team / Agent Activity
```

---

# 8. Product Area acceptance criteria

## PA-01

**AC-PA01-001** User tạo được Project Template có nested folder/document structure tương đương `docs/sample-project/`.  
**AC-PA01-002** User tạo/version/publish được Document Templates.  
**AC-PA01-003** User định nghĩa được semantic/relation policy.  
**AC-PA01-004** User tạo được Task/Tasklist Template có dependencies, context rules và DoR/DoD.  
**AC-PA01-005** Invalid template không publish được.

## PA-02

**AC-PA02-001** Project instantiate từ exact template version và preserve provenance.  
**AC-PA02-002** Runtime structure CRUD không làm mất stable identity.  
**AC-PA02-003** Requirement có thể drill-down qua design/deliverable/task/verification.  
**AC-PA02-004** Relation CRUD/query hoạt động trên generic traceable entities.  
**AC-PA02-005** Coverage engine chỉ ra được missing items cụ thể.  
**AC-PA02-006** Verification pin được exact implementation/entity version.  
**AC-PA02-007** Baseline/change/impact không nhầm structural change với semantic change.

## PA-03

**AC-PA03-001** Human và AI Agent đều nhận/execute cùng Task model.  
**AC-PA03-002** `TaskId` resolve được deterministic context bundle có exact version refs.  
**AC-PA03-003** Task result/artifact/evidence auditable.  
**AC-PA03-004** Requirement Completion Dashboard trả lời được requirement nào complete/incomplete và vì sao.  
**AC-PA03-005** Project Dashboard phân biệt task progress với requirement/deliverable/verification progress.  
**AC-PA03-006** Blocker/review/handoff giữa AI và human được quản lý explicit.

---

# 9. MVP roadmap and dependencies

```text
MVP-1  PA-01 Template foundation
       C01 Project Template
       C02 Structure Template
       C03 Document Template
       C06 Task/Tasklist Template
       basic C04/C07 validation policies

MVP-2  PA-02 Project Instantiation & Workspace
       C01 Project Instantiation
       C02 Structure/Documents
       C03 Semantic Objects

MVP-3  PA-02 Traceability & Coverage
       C04 Requirement Drill-down
       C05 Relations
       C06 Deliverables
       C07 Runtime Tasklist
       C08 Verification
       C09 Coverage Engine

MVP-4  PA-03 Work Management
       C01 Task Board
       C02 Human/AI Assignment
       C03 Task Context
       C04 Result/Evidence
       C05 Review/Blocker/Handoff

MVP-5  PA-03 Dashboard
       C06 Project Progress
       C07 Requirement Completion
       C08 Team/Agent Activity

MVP-6  PA-02 Change/Impact + Platform Integration
       C10 Baseline/Change/Impact
       PF-04 API
       PF-05 Export/Sync
```

Dependency rule:

```text
PA-01 methodology definition
        ↓
PA-02 runtime project graph
        ↓
PA-03 execution/dashboard
```

Không nên build dashboard trước khi coverage/query semantics của PA-02 được xác định, vì nếu không dashboard sẽ chỉ trở thành một task counter giống các project-management app thông thường.

---

# 10. Core product success condition

Sản phẩm đạt core differentiation khi một user có thể:

1. define/publish một software methodology bằng template gồm document structure, semantic/relation rules và tasklist templates;
2. instantiate một project thật từ exact template version;
3. quản lý requirement/design/deliverable/task/verification bằng stable IDs và typed relations;
4. biết một requirement đã drill-down/implementation/verification đến đâu và còn thiếu cụ thể cái gì;
5. giao một task cho human hoặc AI với deterministic context + write scope + acceptance/verification contract;
6. nhận result/artifact/evidence và tính completion từ project graph thay vì chỉ tin rằng agent nói “done”;
7. quan sát project progress theo requirement, design, deliverable, task và verification;
8. phát hiện stale/impact khi upstream requirement hoặc design thay đổi.