# Design, Deliverable, Task và Traceability Governance

## 1. Mục đích

Tài liệu này định nghĩa governance từ Requirement đến Design, Deliverable, Task và Verification. Nó không định nghĩa folder/document hierarchy; project structure được quản lý riêng bằng `ProjectStructureNode` như mô tả trong `APP-DOMAIN-MODEL.md` và `docs/DOCUMENT-ENTITY-MODEL.md`.

Mục tiêu là để project trả lời được bằng structured data:

- requirement nào sinh ra output này;
- design nào quy định contract của output;
- task nào tạo/thay đổi output;
- task phải đọc exact object/version nào;
- verification nào chứng minh requirement/output;
- dependency nào chặn planning;
- khi upstream thay đổi thì downstream nào cần review/update/revalidation/replan.

Canonical chain:

```text
Goal / Business Flow
        ↓
Requirement / Rule / Acceptance Criterion
        ↓
Design Decision / Design Specification
        ↓
Deliverable
        ↓
Task
        ↓
Implementation Artifact
        ↓
Verification Run / Evidence
```

Traceability là graph; chain trên chỉ là path điển hình.

## 2. Canonical concepts

### 2.1 Requirement

Requirement mô tả **WHAT must be true**. Requirement không tự quyết định controller, table hoặc library trừ khi đó là explicit constraint.

### 2.2 Document

Document là authoring container. Một document có thể chứa/render nhiều semantic objects qua `KnowledgePlacement`. Document có thể tham gia traceability khi có semantic relation có ý nghĩa, nhưng vị trí folder của document không phải relation.

### 2.3 Design Decision

Design Decision ghi lại lựa chọn và rationale: boundary, trade-off, pattern, ownership, constraint hoặc technical direction.

### 2.4 Design Specification

Design Specification là traceable semantic object quy định contract/hình dạng của một deliverable. Đây là tên canonical thay cho cách gọi mơ hồ `Design Product` khi nói về entity trong app.

Ví dụ:

```text
SCR-P2P-003-SPEC  Screen Specification
API-P2P-007-SPEC  API Contract
JOB-P2P-001-SPEC  Job Specification
EVT-P2P-004-SPEC  Event Contract
DATA-P2P-001-SPEC Data Specification
```

Một document design có thể render nhiều DesignSpecifications.

### 2.5 Deliverable

Deliverable là output mà project quyết định phải tồn tại hoặc được cung cấp.

Initial types:

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

Deliverable khác source file/commit/PR. Những thứ sau là `ImplementationArtifact`.

### 2.6 Task

Task là execution contract để tạo/sửa/remove/verify một declared target. Task không được tự phát minh deliverable mới khi đã Ready; discovery làm thay đổi scope phải đi qua change process.

### 2.7 Verification

Tách:

```text
VerificationDefinition
  ↓ executed-as
VerificationRun
  ↓ produces
Evidence
```

Definition là specification của việc kiểm chứng; Run/Evidence là bằng chứng thực thi cụ thể.

## 3. Canonical relation vocabulary

Quan hệ phải có một direction canonical. Reverse label chỉ là generated view.

```text
Goal/Flow      --decomposes-to--> Requirement / lower semantic object
Requirement    --governed-by----> BusinessRule / Policy / Standard
Requirement    --accepted-by----> AcceptanceCriterion
Requirement    --satisfied-by---> DesignDecision / DesignSpecification
Requirement    --requires-------> Deliverable
DesignSpec     --specifies------> Deliverable
Task           --implements-----> Deliverable
Task/Result    --produces-------> ImplementationArtifact
VerificationDefinition --verifies--> Requirement / Deliverable
Task/Deliverable/DesignSpec --depends-on--> prerequisite
Document       --references-----> traceable entity when semantically useful
ChangeRequest  --impacts--------> traceable entity
```

Ví dụ chuẩn:

```text
REQ-P2P-012 --requires-------> API-P2P-007
REQ-P2P-012 --satisfied-by---> API-P2P-007-SPEC
API-P2P-007-SPEC --specifies--> API-P2P-007
TASK-P2P-BE-042 --implements--> API-P2P-007
VER-P2P-012 --verifies-------> REQ-P2P-012
VER-P2P-012 --verifies-------> API-P2P-007
TASK-P2P-BE-042 --produces---> PR-381
```

UI có thể hiển thị reverse:

```text
API-P2P-007 required-by REQ-P2P-012
API-P2P-007 specified-by API-P2P-007-SPEC
API-P2P-007 implemented-by TASK-P2P-BE-042
```

Reverse edge không được lưu thành editable canonical edge thứ hai.

## 4. Structure relation không phải traceability relation

Không dùng relation graph để thay project structure tree.

```text
ProjectStructureNode parent-child
→ folder/document navigation

KnowledgePlacement
→ semantic object appears in document

Relation
→ semantic dependency/traceability
```

Ví dụ `30-design/api/API-001.md` nằm trong folder `api` là structural fact. `API-001-SPEC specifies API-001` mới là semantic fact.

## 5. Deliverable Inventory

Deliverable inventory là canonical set các output project quyết định phải có. Một deliverable cần tối thiểu:

```text
ID / Key
Type
Name
Owner
Lifecycle state
Current version
Required-by requirement(s)
Specified-by design specification(s)
Implemented-by task(s)
Verification coverage
```

Các view `required-by`, `specified-by`, `implemented-by` phải derive từ canonical relation graph.

Validation examples:

- Deliverable không có upstream Requirement → coverage gap.
- Deliverable ở `Specified` nhưng thiếu required DesignSpecification → invalid transition.
- Deliverable ở `Implemented` nhưng không có implementing task/artifact → warning/error theo policy.
- Deliverable ở `Verified` nhưng verification target version không current → stale.

## 6. Design Specification Inventory

Không coi design hoàn thành chỉ vì có một file “design.md”. Required specification phụ thuộc DeliverableType.

### Screen

Tối thiểu có thể gồm:

- screen purpose/context;
- layout/regions;
- fields/items;
- actions/interactions;
- validation;
- authorization;
- state/loading/error behavior;
- related API/data contract;
- accessibility/i18n nếu applicable.

### API

- endpoint/operation;
- request/response contract;
- validation;
- authorization;
- errors;
- idempotency;
- transaction/concurrency;
- observability;
- versioning/compatibility.

### Batch/Job

- trigger/schedule;
- selection condition;
- processing algorithm;
- transaction boundary;
- retry/rerun;
- idempotency;
- concurrency;
- partial failure;
- logging/metrics/alerting.

### File/Interface

- producer/consumer;
- timing;
- schema/layout;
- naming;
- encoding;
- transfer/security;
- retry/duplicate handling;
- retention;
- compatibility/versioning.

### Report

- dimensions;
- measures;
- calculation rules;
- cutoff/timezone;
- rounding;
- drilldown;
- export format;
- authorization.

ProjectTemplate có thể định nghĩa mandatory design products/sections theo DeliverableType.

## 7. Task là execution contract

Task phải trả lời năm câu hỏi:

1. Tại sao làm?
2. Phải đọc gì và version nào?
3. Được phép thay đổi gì?
4. Phải tạo/thay đổi deliverable nào?
5. Bằng chứng nào chứng minh hoàn thành?

Conceptual task manifest:

```yaml
key: TASK-P2P-BE-042
title: Implement Purchase Order Approval API
type: backend
milestone: MS-P2P-02
priority: high

objective:
  description: Implement server-side approval behavior.

inputs:
  - entity: REQ-P2P-012
    version: 4
    required: true
  - entity: API-P2P-007-SPEC
    version: 2
    required: true
  - entity: BR-P2P-006
    version: 3
    required: true

targets:
  - entity: API-P2P-007
    action: Modify

verification:
  - AC-P2P-012-01
  - VER-P2P-012

dependsOn:
  - TASK-P2P-DATA-030
```

Allowed repository paths có thể là task execution policy/integration metadata, nhưng không thay canonical Deliverable scope.

## 8. Task lifecycle

Canonical Task lifecycle:

```text
Draft → Ready → InProgress → Review → Done
          ↕          ↕
        Blocked    Blocked
```

`Cancelled` là terminal riêng.

Không dùng `Implemented → Verified → Accepted` cho Task. Đó là Deliverable lifecycle:

```text
Planned → Specified → Implemented → Verified → Accepted → Deprecated
```

`Task Done` chỉ nói execution contract của task đã hoàn thành. `Deliverable Accepted` yêu cầu lifecycle/verification/acceptance riêng.

## 9. Definition of Ready

Implementation task chỉ Ready khi policy-required conditions thỏa mãn, ví dụ:

- upstream Requirement tồn tại và ở trạng thái phù hợp;
- target Deliverable đã được declared;
- mandatory DesignSpecification đủ;
- required inputs có exact version/baseline;
- acceptance/verification requirement rõ;
- dependency đạt required state;
- scope đủ nhỏ và ownership không conflict;
- không có unresolved blocking change.

AI/human không được dùng Ready task như chỗ để tự hoàn thiện requirement/design còn thiếu.

## 10. Definition of Done

Task Done policy có thể yêu cầu:

- declared task targets đã được xử lý;
- required implementation artifacts/result đã submit;
- required checks/tests pass;
- no unapproved scope expansion;
- traceability edges/result data đầy đủ;
- review đã hoàn tất nếu policy yêu cầu.

Done không tự transition Deliverable sang Accepted.

## 11. Dependency layers

Cần phân biệt:

- business dependency;
- design/contract dependency;
- deliverable/runtime dependency;
- task execution dependency.

Không suy luận execution dependency chỉ từ thứ tự file/task.

Ví dụ frontend/backend có thể chạy song song nếu cùng phụ thuộc một API contract đã baseline:

```text
API-P2P-007-SPEC
      /       \
     ↓         ↓
FE Task       BE Task
      \       /
       ↓     ↓
      E2E/Integration
```

`depends-on` direction luôn là consumer → prerequisite.

## 12. Roadmap, Phase, Milestone

```text
Roadmap
  └── Phase
       └── Milestone
            ├── required deliverable outcomes
            └── tasks
```

Milestone nên được định nghĩa bằng outcome/deliverable state, không chỉ danh sách task hoặc target date.

Ví dụ:

```text
MS-P2P-02 complete when:
- SCR-P2P-003 >= Verified
- API-P2P-007 >= Verified
- EVT-P2P-004 >= Verified
```

Task decomposition có thể thay đổi mà milestone outcome không cần đổi.

## 13. Planning sequence

Recommended sequence:

```text
Business Scope / Goal
    ↓
Business Flow / Capability
    ↓
Requirements / Rules / Acceptance Criteria
    ↓
Deliverable Inventory
    ↓
Design Specification Inventory
    ↓
Dependency Graph
    ↓
Task Decomposition
    ↓
Milestones / Roadmap
    ↓
Execution / Verification
```

Không nên tạo tasklist lớn trước khi biết Deliverable Inventory; nếu không AI/human dễ invent architecture/work không có upstream reason.

## 14. Many-to-many Task ↔ Deliverable

Một Task có thể implement nhiều Deliverables nếu chúng cùng một unit of change hợp lý. Một Deliverable có thể cần nhiều Tasks.

Rule thực dụng: Task có một objective thống nhất, scope đủ nhỏ để review/verify và không vượt ownership boundary vô lý.

## 15. Ownership

Requirement, DesignSpecification và Deliverable nên có owner. Ownership được dùng để:

- phân review responsibility;
- validate cross-boundary changes;
- resolve change impact owner;
- map với code ownership/architecture policy khi integration cho phép.

Task của module A không được tự sửa contract thuộc module B nếu chưa có authorized scope/change.

## 16. Version, Baseline và Staleness

TaskInput, version-sensitive Relation hoặc VerificationRun phải có thể pin exact version/revision.

Ví dụ:

```text
TASK-P2P-BE-042 consumed:
- REQ-P2P-012@v4
- API-P2P-007-SPEC@v2
```

Nếu upstream baseline đổi sang v5/v3, system không tự reopen task. Nó đánh dấu stale/potential impact và dùng policy/disposition để quyết định review/revalidation/replan.

## 17. Change control

Khi implementation phát hiện cần deliverable/design/requirement mới ngoài approved scope:

```text
Task discovery
   ↓
ChangeRequest proposal
   ↓
ImpactAnalysis
   ↓
Review / Approve
   ↓
Create/update canonical entities
   ↓
Replan affected tasks/milestones
```

Không âm thầm thêm output mới chỉ vì agent thấy thuận tiện.

## 18. Coverage và validation

System phải query/validate được ít nhất:

- Requirement không có Design/Deliverable.
- Requirement không có AcceptanceCriterion khi policy yêu cầu.
- Deliverable không có upstream Requirement.
- Deliverable không có mandatory DesignSpecification.
- Deliverable không có implementation Task.
- Deliverable không có current verification.
- Task không có upstream reason/target.
- Task Ready thiếu required inputs.
- Acyclic dependency có cycle.
- ImplementationArtifact không trace được về task/result.
- Version-sensitive relation/input stale.
- Orphan Document/KnowledgeObject/Deliverable theo policy.

## 19. Task context for AI/Human tooling

Prompt không nên copy toàn bộ project documentation. Tool/agent nhận Task ID rồi gọi context API để resolve:

```text
Task
+ exact required entity versions
+ related documents
+ target deliverables
+ allowed scope
+ dependency state
+ acceptance/verification requirements
+ current baseline
```

Task context là projection của canonical graph, không phải một source of truth mới.

## 20. Machine-readable principle

Thông tin cần query/validate/diff/impact phải là structured state trong database/API, không chỉ prose:

- identity;
- type;
- lifecycle;
- relation;
- dependency;
- owner;
- version/baseline;
- task input/target;
- verification target;
- impact disposition.

Markdown/rich text phù hợp với rationale, explanation, diagrams và narrative.

## 21. Sample-project role

`docs/sample-project/` không phải canonical product model. Nó là acceptance fixture để kiểm tra rằng application có thể:

- instantiate nested folder/document structure;
- preserve stable identity qua move/rename;
- create/place semantic objects;
- manage Deliverable/Task/Verification;
- create/query typed relations;
- export structure + graph;
- perform impact analysis khi baseline entity thay đổi.

> **Governance đúng không phải “nhiều Markdown”. Nó là identity + version + explicit output + execution contract + typed relation + verification + change history.**