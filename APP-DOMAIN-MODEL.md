# Engineering Governance Platform — Domain Model

## 1. Mục tiêu của domain model

Tài liệu này định nghĩa các object cốt lõi và quan hệ giữa chúng cho ứng dụng quản lý project knowledge, task và output. Mục tiêu là loại bỏ sự nhập nhằng giữa những khái niệm thường bị trộn vào nhau trong project software: file Markdown với requirement, task với deliverable, test với evidence, source file với system output, assignee với AI agent và roadmap với tasklist.

Domain model phải giữ được một nguyên tắc: **project là đối tượng trung tâm; human, AI và service chỉ là member thực hiện work trên các object của project**.

## 2. Bounded Context

Đề xuất tách domain thành các bounded context sau:

```text
Project Governance
├── Project & Membership
├── Knowledge Management
├── Deliverable Management
├── Work Management
├── Traceability
├── Verification
└── Integration
```

Các bounded context có thể nằm trong một modular monolith ở MVP. Việc chia context nhằm giữ ownership và dependency rõ ràng, không phải để ép triển khai microservice.

## 3. Project & Membership

### 3.1 Project

`Project` là aggregate root cấp cao nhất cho governance scope.

```text
Project
- ProjectId
- Key
- Name
- Description
- Status
- OwnerMemberId
- CreatedAt
- UpdatedAt
```

Status ban đầu:

```text
Draft
Active
Archived
```

### 3.2 Member

`Member` đại diện cho một actor có thể tương tác với project.

```text
Member
- MemberId
- ProjectId
- MemberType
- DisplayName
- Status
- Profile
```

`MemberType`:

```text
Human
AI
Service
```

Không tạo `AITask`, `HumanTask` hay lifecycle riêng theo member type. Task phải độc lập với loại assignee.

### 3.3 Role và Permission

```text
Role
- RoleId
- ProjectId
- Name

Permission
- PermissionCode
- Description

MemberRole
- MemberId
- RoleId

RolePermission
- RoleId
- PermissionCode
```

Permission được enforce ở backend/application layer.

## 4. Knowledge Management

### 4.1 Document

`Document` là container phục vụ authoring và navigation.

```text
Document
- DocumentId
- ProjectId
- ParentDocumentId?
- Title
- DocumentType
- Format
- Status
- CurrentVersionId
```

Document có thể là Markdown, rich text hoặc structured editor document. Domain không được giả định một document tương ứng đúng một requirement hay một design object.

### 4.2 DocumentVersion

```text
DocumentVersion
- DocumentVersionId
- DocumentId
- VersionNumber
- Content
- CreatedBy
- CreatedAt
- ChangeSummary
```

Document version khác Knowledge Object version. Một document có thể thay layout/wording mà không làm thay đổi semantic version của mọi object nằm trong đó.

### 4.3 KnowledgeObject

`KnowledgeObject` là semantic object có ID ổn định và có thể tham gia traceability graph.

```text
KnowledgeObject
- KnowledgeObjectId
- ProjectId
- ObjectType
- Key
- Title
- Status
- OwnerMemberId?
- CurrentVersionId
```

Các `ObjectType` ban đầu:

```text
BusinessGoal
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

### 4.4 KnowledgeObjectVersion

```text
KnowledgeObjectVersion
- KnowledgeObjectVersionId
- KnowledgeObjectId
- VersionNumber
- Payload
- CreatedBy
- CreatedAt
- BaselineAt?
```

`Payload` có thể là JSONB theo schema của từng ObjectType. Mỗi type phải có schema validator riêng.

### 4.5 KnowledgePlacement

Để một structured object có thể xuất hiện trong document mà không duplicate semantic source:

```text
KnowledgePlacement
- PlacementId
- KnowledgeObjectId
- DocumentId
- Anchor
- DisplayMode
- SortOrder
```

Document hiển thị object qua placement/reference. Nội dung canonical của metadata cần validate nằm trong KnowledgeObject/Version.

## 5. Deliverable Management

### 5.1 Deliverable

`Deliverable` là output mà project quyết định phải tồn tại.

```text
Deliverable
- DeliverableId
- ProjectId
- Key
- DeliverableType
- Name
- Status
- OwnerMemberId?
- CurrentVersion
```

Types ban đầu:

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
DatabaseArtifact
Configuration
DeploymentArtifact
DocumentationDeliverable
```

### 5.2 Deliverable lifecycle

```text
Planned
  ↓
Specified
  ↓
Implemented
  ↓
Verified
  ↓
Accepted
  ↓
Deprecated
```

State transition phải do domain policy quyết định, không chỉ là field editable trực tiếp.

### 5.3 DeliverableVersion

Deliverable version dùng để đại diện contract/revision của deliverable, không phải source code commit.

```text
DeliverableVersion
- DeliverableVersionId
- DeliverableId
- VersionNumber
- Status
- CreatedAt
- BaselineAt?
```

Ví dụ `API-P2P-007 v3` có thể được implemented bởi nhiều source artifacts ở cùng revision set.

## 6. Work Management

### 6.1 Task

`Task` là đơn vị work được giao cho member.

```text
Task
- TaskId
- ProjectId
- Key
- Title
- TaskType
- Objective
- Status
- Priority
- AssigneeMemberId?
- MilestoneId?
- CreatedBy
- CreatedAt
- UpdatedAt
```

### 6.2 Task lifecycle

```text
Draft
  ↓
Ready
  ↓
InProgress
  ↓
Review
  ↓
Done
```

`Blocked` là trạng thái có thể chuyển vào/ra từ Ready/InProgress/Review. `Cancelled` là terminal state khác Done.

Task không dùng các status `Implemented`, `Verified`, `Accepted`; đó là lifecycle của deliverable.

### 6.3 TaskContext

Không lưu `readSet`, `writeSet`, `verifySet` thành prose duy nhất trong description. Cần object hóa:

```text
TaskInput
- TaskId
- ObjectId
- ObjectType
- RequiredVersion?
- Required: bool

TaskScope
- TaskId
- TargetObjectId
- TargetObjectType
- Action

TaskVerificationRequirement
- TaskId
- VerificationDefinitionId
- Required: bool
```

`TaskInput` có thể trỏ tới KnowledgeObject, Deliverable, Document, Standard hoặc external artifact tùy type policy.

### 6.4 TaskDependency

```text
TaskDependency
- FromTaskId
- ToTaskId
- DependencyType
```

Các dependency type ban đầu:

```text
FinishToStart
ContractBaseline
Information
Environment
```

Không nên ép mọi dependency project thành task-to-task. Deliverable/knowledge dependency nằm trong Traceability context.

## 7. Planning

### 7.1 Roadmap

```text
Roadmap
  └── Phase
       └── Milestone
```

### 7.2 Milestone

```text
Milestone
- MilestoneId
- ProjectId
- PhaseId
- Name
- Status
- TargetDate?
```

Milestone outcome không được tính chỉ bằng task count.

### 7.3 MilestoneOutcome

```text
MilestoneOutcome
- MilestoneId
- DeliverableId
- RequiredState
```

Ví dụ milestone hoàn thành khi ba deliverable đạt `Verified`, bất kể implementation được chia thành bao nhiêu task.

## 8. Traceability

### 8.1 TraceObject

Không nhất thiết tạo bảng `TraceObject` vật lý nếu dùng polymorphic references, nhưng về conceptual model mọi object có thể tham gia graph phải có global project-scoped identity.

Có thể dùng:

```text
ObjectRef
- ObjectType
- ObjectId
```

### 8.2 Relation

```text
Relation
- RelationId
- ProjectId
- FromObjectType
- FromObjectId
- RelationType
- ToObjectType
- ToObjectId
- CreatedBy
- CreatedAt
- Metadata
```

Relation là canonical edge. Reverse edge không lưu riêng.

### 8.3 RelationTypeDefinition

```text
RelationTypeDefinition
- RelationType
- AllowedFromTypes[]
- AllowedToTypes[]
- IsAcyclic
- IsVersionSensitive
- Description
```

Điều này cho phép validator chặn relation vô nghĩa như `Task specifies Requirement` nếu schema không cho phép.

### 8.4 Vocabulary ban đầu

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
executed-as
depends-on
owned-by
assigned-to
contained-in
supersedes
```

## 9. Implementation Artifact

### 9.1 Khái niệm

`ImplementationArtifact` là technical/physical artifact thực tế được tạo ra bởi work.

```text
ImplementationArtifact
- ArtifactId
- ProjectId
- ArtifactType
- ExternalSystem
- Repository
- ExternalId / Path
- Revision
- Checksum?
- Metadata
```

Artifact types có thể gồm:

```text
SourceFile
Commit
PullRequest
MigrationFile
OpenApiDocument
DatabaseMigration
WorkflowDefinition
ContainerImage
DeploymentPackage
GeneratedFile
```

Không đồng nhất `DatabaseMigration` artifact với deliverable `DatabaseArtifact`. Ví dụ table `purchase_orders` là deliverable; migration `20260922_CreatePurchaseOrders.cs` là implementation artifact.

### 9.2 TaskResult

```text
TaskResult
- TaskResultId
- TaskId
- SubmittedBy
- SubmittedAt
- Summary
- Status
```

TaskResult liên kết tới artifact qua relation hoặc explicit join table. Human hoặc AI đều submit cùng schema.

## 10. Verification

### 10.1 VerificationDefinition

```text
VerificationDefinition
- VerificationDefinitionId
- ProjectId
- Key
- Type
- Name
- Definition
```

Types:

```text
UnitTest
IntegrationTest
ContractTest
E2E
ArchitectureTest
StaticAnalysis
SecurityScan
ManualReview
QualityGate
```

### 10.2 VerificationTarget

```text
VerificationTarget
- VerificationDefinitionId
- TargetObjectType
- TargetObjectId
```

Target có thể là Requirement, AcceptanceCriterion, Deliverable hoặc một scope khác được policy cho phép.

### 10.3 VerificationRun

```text
VerificationRun
- VerificationRunId
- VerificationDefinitionId
- TargetRevision
- ExecutorMemberId?
- ExternalRunId?
- Status
- StartedAt
- FinishedAt
```

Status:

```text
Queued
Running
Passed
Failed
Cancelled
Error
```

### 10.4 Evidence

```text
Evidence
- EvidenceId
- VerificationRunId
- EvidenceType
- Location
- Hash?
- Metadata
```

Evidence có thể là test report, CI log, screenshot, static-analysis report hoặc manual review record.

Một `VerificationDefinition` không được tính là evidence. Chỉ `VerificationRun` gắn với target revision cụ thể mới có thể đóng góp vào trạng thái Verified.

## 11. Change & Baseline

### 11.1 Baseline

`Baseline` là snapshot logic của các version được project chấp nhận làm reference.

```text
Baseline
- BaselineId
- ProjectId
- Name
- Type
- CreatedAt
- CreatedBy

BaselineItem
- BaselineId
- ObjectType
- ObjectId
- VersionId
```

### 11.2 ChangeRequest

```text
ChangeRequest
- ChangeRequestId
- ProjectId
- Title
- Reason
- Status
- RequestedBy
- CreatedAt
```

Change Request không bắt buộc cho mọi edit. Nó dùng cho những thay đổi vượt scope hoặc làm thay đổi baseline/contract theo policy project.

### 11.3 Impact

Khi một version baseline thay đổi:

```text
Changed Object
   ↓
Traceability traversal
   ↓
Impacted Objects
   ↓
Impact Assessment
   ↓
Revalidation / Rework / No Impact
```

`ImpactRecord` nên lưu quyết định để có audit trail.

## 12. Các invariant quan trọng

Các invariant sau phải được enforce ở domain/application layer:

1. `Project.Key` unique.
2. Object key unique trong project theo namespace policy.
3. Member type không làm thay đổi Task schema.
4. Một Task chỉ có tối đa một primary assignee tại một thời điểm trong MVP.
5. Relation phải tuân `RelationTypeDefinition`.
6. Reverse relation không được lưu như independent editable edge.
7. Baseline version immutable.
8. Task chỉ được chuyển `Ready` khi Definition of Ready validator pass.
9. Deliverable chỉ được chuyển `Specified` khi mandatory specifications tồn tại.
10. Deliverable chỉ được chuyển `Implemented` khi implementation coverage policy pass.
11. Deliverable chỉ được chuyển `Verified` khi verification policy pass trên current deliverable revision.
12. Deliverable `Accepted` phải thuộc một accepted baseline/release decision.
13. Verification Definition không được dùng trực tiếp làm evidence.
14. Task Done phải có TaskResult hoặc explicit no-output completion reason.
15. Không cho cycle ở relation type được định nghĩa `IsAcyclic = true`.

## 13. Logical data model tối thiểu cho MVP

Một relational implementation trên PostgreSQL có thể bắt đầu với các bảng:

```text
projects
members
roles
permissions
member_roles
role_permissions

documents
document_versions
knowledge_objects
knowledge_object_versions
knowledge_placements

deliverables
deliverable_versions

tasks
task_inputs
task_scopes
task_dependencies
task_results

phases
milestones
milestone_outcomes

relations
relation_type_definitions

implementation_artifacts

verification_definitions
verification_targets
verification_runs
evidence

baselines
baseline_items
change_requests
impact_records
```

Không cần graph database ở MVP. Với indexes trên `(project_id, from_object_type, from_object_id)` và `(project_id, to_object_type, to_object_id)`, PostgreSQL recursive CTE đủ cho traversal ban đầu.

## 14. Module dependency đề xuất

```text
ProjectMembership
      ↑
Knowledge      Deliverables
      \          /
       Traceability
          ↑
       WorkManagement
          ↑
       Verification
          ↑
       Integration
```

Cách dependency thực tế nên được giữ qua application contracts/domain references thay vì shared database access tùy tiện.

Một cách modular-monolith dễ kiểm soát:

```text
src/
  ProjectMembership/
  Knowledge/
  Deliverables/
  WorkManagement/
  Traceability/
  Verification/
  Integration/
```

Mỗi module có Application, Domain và Infrastructure riêng nếu dùng Clean Architecture/modular architecture.

## 15. AI integration nằm ở đâu

AI không phải bounded context cốt lõi. Nó nằm trong Integration và Membership.

```text
Member(type=AI)
        ↓ assigned-to
      Task
        ↓
Task Execution Port
        ↓
AI Executor Adapter
        ↓
TaskResult + ImplementationArtifact
```

Core domain chỉ biết member được phép execute task và đã submit TaskResult. Model/provider/prompt/runtime là concern của adapter/profile.

Nhờ vậy có thể thay:

```text
OpenAI
Claude
Local model
Human developer
External automation
```

mà không thay Task, Deliverable hoặc Traceability domain.

## 16. Kết luận

Domain model phải bảo vệ ba separation quan trọng nhất:

```text
Knowledge ≠ Document
Work ≠ Deliverable
Verification Definition ≠ Verification Evidence
```

và một nguyên tắc xuyên suốt:

```text
Human / AI / Service
       ↓
     Member
       ↓
   performs Task
       ↓
changes Project Objects
```

Ứng dụng vì vậy không phải một AI coding orchestrator. Nó là project governance platform có structured knowledge, planning, traceability, output inventory và verification; AI chỉ là một trong các member có thể tham gia thực thi work trong hệ thống đó.
