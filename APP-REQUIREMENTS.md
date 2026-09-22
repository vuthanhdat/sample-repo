# Software Project Governance SaaS — Product Requirements

## 1. Product Definition

Sản phẩm là một **SaaS quản lý dự án phần mềm theo hướng traceability-first**. Core value không phải là lưu nhiều file Markdown, mà là quản lý có cấu trúc toàn bộ project knowledge, document structure, requirement, design, deliverable/output, task, verification, version, change và quan hệ giữa chúng để có thể query coverage và impact một cách máy móc.

Sản phẩm phải cho phép user tạo một project, CRUD một cây folder/document tương tự một repository documentation tree, tạo các semantic/software-project objects bên trong project, liên kết chúng bằng typed relations và đánh giá ảnh hưởng khi một entity thay đổi.

`docs/sample-project/` trong repository này chỉ là **reference/acceptance fixture**. Nó minh họa một cấu trúc tài liệu mà app phải có thể tạo/quản lý/export; nội dung sample không phải requirement source của app.

Sản phẩm không thay thế source control, CI/CD hay issue tracker. SaaS là system of record cho governance/semantic project state; repository là system of record cho source code; CI provider là system of record cho CI execution result.

## 2. Core mental model

Ứng dụng phải tách hai cấu trúc:

```text
Project Structure Tree                     Traceability Graph
----------------------                     ------------------
Folder / Document hierarchy                Typed relations among traceable entities
navigation / ordering / export path        dependency / coverage / impact / backlinks

Project                                    Goal / Flow / Requirement / Document
└── Folder                                 Design / Deliverable / Task / Verification
    ├── Document                           Artifact / Milestone / Change
    └── Folder                                         ↕
        └── Document                                  Relation
```

Folder/document parent-child **không phải** semantic relation. Move/rename/reorder một node chỉ thay đổi structure/navigation, trừ khi một explicit policy nói export path là contract.

## 3. SaaS và tenancy

### 3.1 User

`User` là human SaaS account. Một user có thể tạo nhiều project, tham gia project của người khác và có role khác nhau ở từng project.

### 3.2 Project

`Project` là boundary chính của dữ liệu. Structure nodes, documents, semantic objects, deliverables, tasks, relations, milestones, baselines, verification, change requests và memberships đều thuộc một project.

Mọi query/command phải enforce project boundary. Cross-project relation mặc định bị cấm.

### 3.3 Principal và membership

Actor trong project:

```text
Principal
├── Human        → linked to User
├── AIAgent      → machine principal
└── Service      → machine principal
        ↓
ProjectMembership
        ↓
Role / Permission
```

Task assignee trỏ tới `ProjectMembership`, không trỏ trực tiếp `User`. AI không có task schema riêng.

## 4. Project Template, Structure Template và Document Template

### 4.1 Project Template

`ProjectTemplateVersion` định nghĩa baseline policy khi khởi tạo project:

- structure template tree;
- document templates bắt buộc/tùy chọn;
- enabled traceable entity types;
- ID/key rules;
- relation vocabulary;
- lifecycle/validation policy;
- required design products theo deliverable type;
- Definition of Ready/Done;
- role presets;
- export layout.

Project phải giữ reference tới exact template version đã dùng.

### 4.2 Structure Template Tree

Project template phải biểu diễn được cả folder và document nodes:

```text
ProjectStructureTemplateNode
├── FolderTemplateNode
└── DocumentTemplateNode → DocumentTemplateVersion
```

Khi instantiate project, template nodes sinh ra `ProjectStructureNode` thực tế. Template chỉ là nguồn khởi tạo/policy, không phải nơi lưu runtime project state.

### 4.3 Document Template

`DocumentTemplateVersion` định nghĩa structure/content cho một document, ví dụ Requirement Specification, API Specification, Screen Specification, Test Strategy.

Template có thể định nghĩa required sections, optional sections, structured placeholders, allowed embedded object types và validation rules.

## 5. Project Structure Tree — functional requirements

### 5.1 Node types

MVP có hai node type:

```text
Folder
Document
```

Folder không phải Document. Document node trỏ tới đúng một `Document`. Một Document trong MVP có tối đa một primary structure node; alias/reference node có thể bổ sung sau.

### 5.2 CRUD operations

User có quyền phải có thể:

- create root/sub-folder;
- create document node từ blank hoặc DocumentTemplateVersion;
- rename node;
- move node sang parent khác;
- reorder siblings;
- archive/restore node;
- delete draft/unreferenced node theo policy;
- copy structure subtree từ template khi được phép;
- query tree hoặc subtree;
- resolve canonical path hiện tại.

Move/rename node không làm đổi `DocumentId`, `Document.Key` hoặc semantic object IDs bên trong document.

### 5.3 Delete policy

Hard delete chỉ áp dụng cho object chưa baseline và không có reference/audit requirement. Với object đã baseline hoặc được reference, hệ thống phải archive/deprecate/supersede theo lifecycle thay vì xóa mất lịch sử.

## 6. Document & Knowledge Management

### 6.1 Document

Document là authoring container và là một traceable entity. User phải có thể:

- create blank/from template;
- edit content;
- manage sections;
- save version/history;
- compare versions;
- baseline/supersede theo permission;
- search theo text/key/type/status/owner;
- view placements/backlinks/relations;
- archive/restore.

### 6.2 Document version và semantic version tách biệt

Document wording/layout thay đổi có thể tạo `DocumentVersion` mà không làm tăng version của mọi semantic object được render trong document.

Ngược lại, khi Requirement đổi business meaning, phải tạo semantic object version mới dù surrounding document content có thể gần như không đổi.

### 6.3 Knowledge Object

Initial structured types:

- Goal;
- BusinessCapability;
- BusinessFlow;
- Requirement;
- BusinessRule;
- AcceptanceCriterion;
- DesignDecision;
- DesignSpecification;
- ArchitectureRule;
- DataSpecification;
- InterfaceContract;
- Policy;
- Standard.

Một KnowledgeObject có stable ID/key và version riêng.

### 6.4 Knowledge Placement

Một KnowledgeObject có thể được render/reference ở nhiều document mà không duplicate canonical state.

```text
KnowledgeObject
   ├── Placement in Requirement Document
   ├── Placement in API Design Document
   └── Placement in Test Document
```

Placement là authoring concern, không phải semantic traceability edge.

## 7. Global traceable entities

Traceability Graph không được giới hạn ở `KnowledgeObject`. Tối thiểu các entity sau phải có thể là relation endpoint:

- Document;
- KnowledgeObject;
- Deliverable;
- Task;
- VerificationDefinition;
- Milestone/Phase khi cần planning relation;
- ImplementationArtifact;
- ChangeRequest;
- ProjectBaseline/ExportSnapshot nếu policy cần.

Mỗi traceable entity có technical ID immutable. Entity được giao tiếp thường xuyên với user phải có project-scoped stable human key.

## 8. Traceability Relation Management

### 8.1 Relation CRUD

User/service có permission phải có thể:

- create relation;
- validate source/target/type;
- update allowed metadata;
- delete relation theo lifecycle/audit policy;
- query outbound relations;
- query inbound/reverse relations;
- query transitive graph với depth/type filters;
- query backlinks;
- inspect relation provenance/created-by;
- inspect version sensitivity/staleness.

Reverse relation là generated view, không lưu editable duplicate edge.

### 8.2 Canonical vocabulary

Initial canonical directions:

```text
decomposes-to  Goal/Capability/Flow/Requirement → lower-level semantic object
governed-by    Requirement/Deliverable           → BusinessRule/Policy/Standard
accepted-by    Requirement                       → AcceptanceCriterion
satisfied-by   Requirement                       → DesignDecision/DesignSpecification
requires       Requirement                       → Deliverable
specifies      DesignSpecification               → Deliverable
implements     Task                              → Deliverable
produces       Task/TaskResult                   → ImplementationArtifact
verifies       VerificationDefinition            → Requirement/Deliverable
depends-on     Task/Deliverable/DesignSpec       → prerequisite entity
references     Document/traceable entity         → traceable entity
supersedes     traceable identity/version         → older identity/version where applicable
impacts        ChangeRequest/ImpactItem           → traceable entity
```

Reverse labels như `required-by`, `specified-by`, `implemented-by`, `verified-by` được generated.

`contained-in` không dùng để mô tả folder/document hierarchy; structure tree có model riêng.

### 8.3 Relation type definition

Mỗi relation type định nghĩa ít nhất:

- canonical name;
- reverse display name;
- allowed source types;
- allowed target types;
- acyclic policy;
- version sensitivity;
- impact propagation mode;
- impact direction;
- optional cardinality/uniqueness constraints.

## 9. Goal → Design → Deliverable → Task model

Typical chain:

```text
Goal
  ↓ decomposes-to
Business Flow / Capability
  ↓ decomposes-to
Requirement
  ├── governed-by → Business Rule
  ├── accepted-by → Acceptance Criterion
  ├── satisfied-by → Design
  └── requires → Deliverable

Design Specification
  └── specifies → Deliverable

Task
  └── implements → Deliverable

Verification Definition
  └── verifies → Requirement / Deliverable
```

App phải hỗ trợ drill-down và reverse trace nhưng canonical model vẫn là graph, không ép thành một cây duy nhất.

Coverage queries tối thiểu:

- requirement chưa có design;
- requirement chưa có deliverable;
- deliverable chưa có specification;
- deliverable chưa có task;
- deliverable chưa được verify;
- task không có upstream reason;
- orphan traceable entity;
- stale relation/input.

## 10. Deliverable / Output Management

Initial deliverable types:

- Screen;
- API;
- BatchJob;
- Event;
- File;
- Report;
- Notification;
- Interface;
- DataObject;
- DatabaseObject;
- Configuration;
- DeploymentArtifact;
- DocumentationDeliverable.

Deliverable có ID/key, type, owner, lifecycle, version và relations tới requirement/design/task/verification.

Lifecycle:

```text
Planned → Specified → Implemented → Verified → Accepted → Deprecated
```

`Specified` yêu cầu mandatory design product theo policy. `Verified` yêu cầu valid verification run trên target version/revision.

## 11. Task & Planning

### 11.1 Task CRUD

App hỗ trợ:

- task create/update/archive/cancel;
- assignment;
- priority;
- milestone/phase;
- task dependency;
- read set / required context;
- write scope / target deliverables;
- acceptance criteria;
- verification requirements;
- result/evidence;
- list/board/dependency views.

Task lifecycle:

```text
Draft → Ready → InProgress → Review → Done
          ↕          ↕
        Blocked    Blocked
```

`Cancelled` là terminal state riêng. `Implemented/Verified/Accepted` là trạng thái của Deliverable, không phải Task.

### 11.2 Definition of Ready

Task không được Ready nếu policy-required context còn thiếu, dependency chưa đạt required state, target deliverable chưa được declare hoặc mandatory design/acceptance criteria chưa baseline/approved theo policy.

### 11.3 Task Context

System phải resolve exact task context từ graph/input bindings thay vì yêu cầu AI/human tự crawl project:

- task metadata/version;
- required semantic object versions;
- related documents;
- target deliverables;
- allowed write scope;
- acceptance criteria;
- verification requirements;
- dependencies;
- current project baseline.

## 12. Roadmap & Milestone

Roadmap/Phase/Milestone quản lý planning outcome. Milestone progress phải có thể dựa trên required deliverable state, không chỉ task count.

```text
Roadmap
  └── Phase
       └── Milestone
            ├── required deliverable outcomes
            └── tasks
```

## 13. Verification & Evidence

Tách rõ:

```text
VerificationDefinition
   ↓ executed-as
VerificationRun
   ↓ produces
Evidence
```

Test definition không tự chứng minh output đã pass. Run phải gắn với target entity/version hoặc implementation revision cụ thể.

## 14. Version, Baseline và Snapshot

### 14.1 Baseline

Baseline cố định tập exact entity versions tại một thời điểm. Baseline version immutable.

### 14.2 Edit baseline object

Không silently overwrite baseline object. Edit semantic baseline tạo version mới và có thể trigger impact analysis.

### 14.3 Staleness

Consumer có version-sensitive dependency/input phải record exact input version hoặc baseline. Khi upstream current/baseline version thay đổi, hệ thống phải xác định consumer có stale hay không theo policy.

## 15. Change Management & Impact Analysis

### 15.1 Change Request

ChangeRequest lifecycle:

```text
Draft → ImpactAnalysis → Review → Approved → Applying → Verified → Closed
                       ↘ Rejected
```

### 15.2 Impact discovery

Impact engine traverse Traceability Graph từ changed entity theo relation policy. Potential impact phải bao gồm được:

- Documents;
- Knowledge Objects;
- Design Specifications;
- Deliverables;
- Tasks;
- Verification Definitions/Runs;
- Milestones/Releases;
- relevant snapshots/exports.

### 15.3 Impact disposition

Graph discovery không tự động đồng nghĩa downstream phải sửa. Mỗi impact item được disposition:

```text
UpdateRequired
ReviewRequired
RevalidationRequired
ReplanRequired
NoChangeRequired
Obsolete
```

Disposition phải có actor, timestamp và rationale khi policy yêu cầu.

### 15.4 Structural change vs semantic change

Move/rename/reorder folder/document node mặc định không propagate semantic impact. Document content/version change, semantic object version change, relation change, deliverable contract change hoặc policy-defined export path change có thể trigger impact.

## 16. External Integration & Export

SaaS là canonical governance state. User có thể export project/baseline/subtree/task-context bundle thành machine-readable + human-readable projection.

Export phải giữ:

- project identity;
- entity IDs/keys;
- exact versions;
- structure paths;
- relation set;
- baseline/snapshot ID;
- schema version;
- checksums khi cần.

Two-way sync chỉ apply qua version comparison, conflict detection và reviewed proposal/change flow; không overwrite canonical state trực tiếp.

## 17. API-first requirements

Mọi capability quan trọng phải có application/API use case tương ứng. Tối thiểu:

```text
GET/POST/PATCH project structure nodes
GET/POST/PATCH documents and semantic objects
GET/POST/DELETE relations
GET graph/backlinks/coverage/impact
GET/POST deliverables
GET/POST tasks and task transitions
GET task context
POST verification runs/evidence
POST change requests / impact dispositions
POST export/sync proposals
```

Status/lifecycle-sensitive change dùng command semantics thay vì generic raw status patch.

## 18. Machine authentication

AI/service không dùng human credential. Machine principal có credential riêng, project membership và scopes.

MVP token requirements:

- high entropy;
- plaintext shown once;
- server stores hash;
- expiry optional;
- revoke/rotate;
- project scoped;
- auditable actor;
- least privilege scopes.

## 19. Key Screens

### SCR-001 SaaS Home
Project list, recent activity, health summary.

### SCR-002 Create Project
Template/version selection, name/key, members.

### SCR-003 Project Overview
Coverage, deliverable health, task progress, stale entities, changes, verification failures.

### SCR-004 Project Structure & Document Explorer
Folder/document tree CRUD, move/reorder, create from template, document authoring, history, placements, backlinks.

### SCR-005 Traceability Explorer
Graph/hierarchy/matrix, inbound/outbound relations, reverse trace, filters, orphan/coverage gaps.

### SCR-006 Deliverable Inventory / Detail
Lifecycle, version, upstream requirement/design, implementing tasks, artifacts, verification.

### SCR-007 Task Board / Task Detail
Planning/execution, context, scope, dependency, result.

### SCR-008 Roadmap / Milestone
Phase/milestone/outcome/task planning.

### SCR-009 Change & Impact Center
Change request, changed versions, potential impact paths, disposition, revalidation/replan.

### SCR-010 Verification Center
Definition/run/evidence, target version, stale/failure views.

### SCR-011 Integration & API Access
Repository binding, exports, machine principals, credentials, webhooks/sync.

## 20. Validation Rules

Minimum rules:

1. Technical ID immutable.
2. Human key unique trong project đối với key-bearing traceable entities.
3. Structure node parent phải cùng project; tree không có cycle.
4. Document node phải reference valid Document cùng project.
5. Move/rename node không đổi Document identity.
6. Cross-project relation bị cấm mặc định.
7. Relation source/target phải đúng `RelationTypeDefinition`.
8. Reverse edge không lưu editable duplicate.
9. Baseline version immutable; edit tạo revision/version mới.
10. Task Ready phải thỏa Definition of Ready.
11. Deliverable Specified phải đủ mandatory design coverage.
12. Deliverable Verified phải có required verification pass trên target version/revision.
13. Acyclic relation/dependency type không được tạo cycle.
14. External lifecycle update phải permission-check + optimistic concurrency.
15. Import/sync không overwrite newer canonical version without resolution.
16. Baseline semantic change phải trigger impact nếu relation policy yêu cầu.
17. Structural-only change không được tạo semantic impact giả.
18. Relation/delete/archive phải giữ audit/provenance cần thiết.

## 21. MVP Roadmap

### MVP-1 — Foundation + Structure

- Authentication.
- Multi-project.
- Membership/RBAC.
- Project template/version.
- Structure template tree.
- Folder/document tree CRUD.
- Document template + document CRUD/version.
- Basic search.

### MVP-2 — Traceable Project Model

- Knowledge objects.
- Deliverables.
- Tasks/milestones.
- Generic relation CRUD.
- Backlinks/graph/matrix.
- Coverage/orphan validation.

### MVP-3 — Baseline & Impact

- Baseline/snapshot.
- Version diff.
- Change request.
- Graph impact traversal.
- Impact disposition.
- Stale/revalidation/replan handling.

### MVP-4 — External Integration

- Export bundle.
- Repository binding.
- Implementation artifact links.
- Verification ingestion.
- Webhook/outbox where useful.

### MVP-5 — Machine Worker

- Machine principal/token.
- Task context API.
- Task result/artifact/evidence submission.
- Agent audit/activity.

## 22. Non-functional Requirements

- API-first.
- Server-side tenant/project isolation.
- Optimistic concurrency for versions/transitions/relation-sensitive updates.
- Idempotency for external commands with side effects.
- Audit trail for permission, baseline, relation, lifecycle, change and machine actions.
- Full-text search + structured filters.
- Practical graph traversal for projects with tens of thousands of traceable entities.
- Schema-versioned import/export.
- PostgreSQL as canonical persistence for MVP; edge table + indexes + recursive CTE before graph database.
- External integration outage must not block core project authoring/governance.

## 23. Product Acceptance Criteria

Product model được coi là đủ rõ để bước sang detailed design/code khi implementation có thể đáp ứng các scenario sau:

### AC-PROD-001 — Recreate sample structure
User định nghĩa/import một project template và instantiate được structure tree tương đương `docs/sample-project/`, gồm nested folders và documents.

### AC-PROD-002 — Structure CRUD preserves identity
User move/rename/reorder một document trong tree; document key/id và semantic object IDs bên trong không đổi.

### AC-PROD-003 — Semantic object placement
Một BusinessRule được canonicalize một lần nhưng render ở nhiều document qua placements.

### AC-PROD-004 — Generic relation CRUD
User tạo relation giữa Requirement→Deliverable, DesignSpecification→Deliverable, Task→Deliverable, VerificationDefinition→Requirement/Deliverable và Document→traceable entity bằng allowed relation type; reverse view query được mà không duplicate edge.

### AC-PROD-005 — Coverage
System phát hiện requirement thiếu design/deliverable, deliverable thiếu spec/task/verification và task thiếu upstream reason.

### AC-PROD-006 — Impact analysis
Khi baseline Requirement tạo version mới, system traverse relation policy và hiển thị potential impact tới relevant document/design/deliverable/task/verification/milestone; user disposition từng impact.

### AC-PROD-007 — Structural change is not semantic change
Move document sang folder khác không tự tạo impact tới Requirement/Task nếu không có path-sensitive policy.

### AC-PROD-008 — Reproducible export
System export project snapshot giữ được structure, IDs, exact versions, relations và baseline metadata để human/AI có thể resolve đúng context.

> **Sản phẩm là một project-governance system có authoring tree + traceability graph. Tree trả lời “tài liệu nằm ở đâu”; graph trả lời “vì sao nó tồn tại, phụ thuộc gì, ai tạo/thay đổi nó và thay đổi này ảnh hưởng tới đâu”.**