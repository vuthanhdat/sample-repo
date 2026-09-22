# Software Project Governance SaaS — Domain Model

## 1. Mục tiêu

Domain model này định nghĩa core objects cho một SaaS quản lý dự án phần mềm theo hướng traceability-first. Hệ thống phải quản lý được project knowledge, document, deliverable/output, task, change, verification và integration mà không phụ thuộc vào việc người thực hiện là human hay AI.

Nguyên tắc chính:

1. SaaS account/user và project membership là hai khái niệm khác nhau.
2. Project là boundary chính của governance data.
3. Document là authoring container; semantic object có identity riêng.
4. Deliverable là thứ project cần có; task là work để tạo/thay đổi nó.
5. Relation là first-class canonical data.
6. Baseline object thay đổi phải có version và impact analysis.
7. External repository/folder là projection/integration, không phải source of truth song song không kiểm soát.
8. Human, AI Agent và Service Account đều là principals có thể tham gia project qua permission.

## 2. Bounded Contexts

```text
Identity & SaaS
Project Governance
Template Management
Knowledge & Documents
Deliverable Management
Work & Planning
Traceability
Change Management
Verification
Integration & Sync
Audit
```

MVP có thể implement dưới dạng modular monolith. Bounded context dùng để phân responsibility, không bắt buộc microservice.

## 3. Identity & SaaS

### 3.1 User

`User` là human SaaS account toàn cục.

```text
User
- UserId
- Email
- DisplayName
- Status
- CreatedAt
```

Một User có thể tạo hoặc tham gia nhiều Project.

### 3.2 Principal

Mọi actor có thể authenticate/act lên API được biểu diễn bởi `Principal`.

```text
Principal
- PrincipalId
- PrincipalType: Human | AIAgent | Service
- DisplayName
- Status
- Metadata
```

Human principal liên kết tới `User`. AI/Service không cần SaaS login session như human.

### 3.3 ProjectMembership

```text
ProjectMembership
- ProjectMembershipId
- ProjectId
- PrincipalId
- Status
- JoinedAt
```

Task assignee trỏ tới ProjectMembership, không trỏ trực tiếp User hay AI-specific entity.

### 3.4 Role / Permission

```text
Role
- RoleId
- ProjectId
- Name

Permission
- PermissionCode
- Description

MembershipRole
- ProjectMembershipId
- RoleId

RolePermission
- RoleId
- PermissionCode
```

Permission được enforce backend-side.

## 4. Project

### 4.1 Project

```text
Project
- ProjectId
- Key
- Name
- Description
- Status: Draft | Active | Archived
- OwnerPrincipalId
- ProjectTemplateVersionId?
- CreatedAt
- UpdatedAt
```

`ProjectId` là technical immutable ID. `Key` là human-readable project key.

### 4.2 ProjectBaseline

```text
ProjectBaseline
- ProjectBaselineId
- ProjectId
- Key
- Name
- CreatedBy
- CreatedAt
- Status
```

Baseline cố định tập version của các object quan trọng tại một thời điểm.

### 4.3 ProjectBaselineItem

```text
ProjectBaselineItem
- ProjectBaselineId
- ObjectType
- ObjectId
- ObjectVersionId
```

Baseline dùng cho reproducible export, release, audit và impact comparison.

## 5. Template Management

### 5.1 ProjectTemplate

```text
ProjectTemplate
- ProjectTemplateId
- Key
- Name
- Description
- Status
```

### 5.2 ProjectTemplateVersion

```text
ProjectTemplateVersion
- ProjectTemplateVersionId
- ProjectTemplateId
- Version
- SchemaVersion
- Definition
- PublishedAt?
```

Definition có thể chứa document tree, object type policy, ID rules, relation vocabulary, validation policy, roles và export layout.

### 5.3 DocumentTemplate

```text
DocumentTemplate
- DocumentTemplateId
- Key
- Name
- DocumentType
- Status
```

### 5.4 DocumentTemplateVersion

```text
DocumentTemplateVersion
- DocumentTemplateVersionId
- DocumentTemplateId
- Version
- StructureDefinition
- ContentTemplate
- PublishedAt?
```

Document template có thể định nghĩa required sections và structured placeholders.

## 6. Knowledge & Documents

### 6.1 Document

```text
Document
- DocumentId
- ProjectId
- Key
- ParentDocumentId?
- Title
- DocumentType
- DocumentTemplateVersionId?
- Status
- CurrentVersionId
- SortOrder
```

Document là authoring/navigation container.

### 6.2 DocumentVersion

```text
DocumentVersion
- DocumentVersionId
- DocumentId
- VersionNumber
- Content
- CreatedByPrincipalId
- CreatedAt
- ChangeSummary
```

Document version độc lập với semantic object version.

### 6.3 KnowledgeObject

```text
KnowledgeObject
- KnowledgeObjectId
- ProjectId
- Key
- ObjectType
- Title
- Status
- OwnerMembershipId?
- CurrentVersionId
```

Initial object types:

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

### 6.4 KnowledgeObjectVersion

```text
KnowledgeObjectVersion
- KnowledgeObjectVersionId
- KnowledgeObjectId
- VersionNumber
- Payload
- CreatedByPrincipalId
- CreatedAt
- BaselineAt?
```

Payload có thể dùng JSONB với schema per ObjectType.

### 6.5 KnowledgePlacement

```text
KnowledgePlacement
- PlacementId
- KnowledgeObjectId
- DocumentId
- Anchor
- DisplayMode
- SortOrder
```

Một object có thể xuất hiện trong nhiều document mà không duplicate canonical semantic state.

## 7. Deliverable Management

### 7.1 Deliverable

```text
Deliverable
- DeliverableId
- ProjectId
- Key
- DeliverableType
- Name
- Status
- OwnerMembershipId?
- CurrentVersionId
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
DatabaseObject
Configuration
DeploymentArtifact
DocumentationDeliverable
```

### 7.2 DeliverableVersion

```text
DeliverableVersion
- DeliverableVersionId
- DeliverableId
- VersionNumber
- Payload
- CreatedByPrincipalId
- CreatedAt
- BaselineAt?
```

Deliverable version đại diện contract/revision của output, không phải source commit.

### 7.3 Deliverable lifecycle

```text
Planned → Specified → Implemented → Verified → Accepted → Deprecated
```

State transition phải được application/domain policy validate.

## 8. Work & Planning

### 8.1 Roadmap hierarchy

```text
Roadmap
  └── Phase
       └── Milestone
            └── Tasks / Outcomes
```

### 8.2 Milestone

```text
Milestone
- MilestoneId
- ProjectId
- PhaseId?
- Key
- Name
- Status
- TargetDate?
```

### 8.3 MilestoneOutcome

```text
MilestoneOutcome
- MilestoneId
- DeliverableId
- RequiredState
```

Progress milestone dựa trên outcome/deliverable, không chỉ task count.

### 8.4 Task

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
- AssigneeMembershipId?
- MilestoneId?
- Version
- CreatedByPrincipalId
- CreatedAt
- UpdatedAt
```

Task lifecycle:

```text
Draft → Ready → InProgress → Review → Done
          ↕          ↕
        Blocked    Blocked
```

`Cancelled` là terminal state khác Done.

### 8.5 TaskInput

```text
TaskInput
- TaskId
- ObjectType
- ObjectId
- RequiredObjectVersionId?
- Required
```

### 8.6 TaskScope

```text
TaskScope
- TaskId
- TargetObjectType
- TargetObjectId
- Action
```

Action ví dụ `Create`, `Modify`, `Remove`, `Verify`.

### 8.7 TaskDependency

```text
TaskDependency
- FromTaskId
- ToTaskId
- DependencyType
```

### 8.8 TaskResult

```text
TaskResult
- TaskResultId
- TaskId
- TaskVersion
- SubmittedByPrincipalId
- SubmittedAt
- Summary
- Status
- ExternalExecutionId?
```

Task result giữ actor và task version để audit external/AI update.

## 9. Traceability

### 9.1 Global project-scoped object identity

Conceptually, mọi object tham gia graph có thể được biểu diễn bởi:

```text
ObjectRef
- ProjectId
- ObjectType
- ObjectId
```

### 9.2 Relation

```text
Relation
- RelationId
- ProjectId
- FromObjectType
- FromObjectId
- RelationType
- ToObjectType
- ToObjectId
- CreatedByPrincipalId
- CreatedAt
- Metadata
```

Relation là canonical edge. Reverse edge không lưu editable riêng.

### 9.3 RelationTypeDefinition

```text
RelationTypeDefinition
- RelationType
- AllowedFromTypes[]
- AllowedToTypes[]
- IsAcyclic
- IsVersionSensitive
- ImpactPropagationMode
- Description
```

`ImpactPropagationMode` cho biết change ở source có cần đánh dấu target là potentially impacted hay không.

Initial vocabulary:

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
impacts
```

## 10. Implementation Artifact

```text
ImplementationArtifact
- ArtifactId
- ProjectId
- ArtifactType
- ExternalSystem
- Repository
- PathOrExternalId
- Revision
- Checksum?
- Metadata
- CreatedAt
```

Artifact types:

```text
SourceFile
Commit
PullRequest
MigrationFile
OpenApiDocument
WorkflowDefinition
ContainerImage
DeploymentPackage
GeneratedFile
ExternalDocument
```

Deliverable khác ImplementationArtifact. Ví dụ API là deliverable; controller file, OpenAPI file và PR là implementation artifacts.

### 10.1 TaskResultArtifact

```text
TaskResultArtifact
- TaskResultId
- ArtifactId
```

## 11. Verification

### 11.1 VerificationDefinition

```text
VerificationDefinition
- VerificationDefinitionId
- ProjectId
- Key
- Type
- Name
- Definition
```

### 11.2 VerificationRun

```text
VerificationRun
- VerificationRunId
- VerificationDefinitionId
- TargetObjectType
- TargetObjectId
- TargetObjectVersionId?
- Revision?
- ExecutedByPrincipalId?
- ExecutedAt
- Status
- ExternalRunId?
```

### 11.3 Evidence

```text
Evidence
- EvidenceId
- VerificationRunId
- EvidenceType
- UriOrPayload
- Checksum?
```

Test definition không phải evidence; evidence chỉ xuất hiện từ run cụ thể.

## 12. Change Management

### 12.1 ChangeRequest

```text
ChangeRequest
- ChangeRequestId
- ProjectId
- Key
- Title
- Reason
- SourceType
- SourceReference?
- Status
- OwnerMembershipId?
- CreatedByPrincipalId
- CreatedAt
```

Lifecycle:

```text
Draft → ImpactAnalysis → Review → Approved → Applying → Verified → Closed
                       ↘ Rejected
```

### 12.2 ChangeItem

```text
ChangeItem
- ChangeItemId
- ChangeRequestId
- TargetObjectType
- TargetObjectId
- FromVersionId?
- ProposedPayloadOrPatch
- ChangeType
```

### 12.3 ImpactItem

```text
ImpactItem
- ImpactItemId
- ChangeRequestId
- ObjectType
- ObjectId
- CurrentVersionId?
- ImpactPath
- ImpactType
- Disposition
- Rationale?
- OwnerMembershipId?
- Status
```

Disposition:

```text
UpdateRequired
ReviewRequired
RevalidationRequired
ReplanRequired
NoChangeRequired
Obsolete
```

### 12.4 Staleness

Version-sensitive relation/input cần có khả năng xác định rằng consumer vẫn trỏ version cũ sau khi upstream baseline thay đổi.

Hệ thống không bắt buộc tự sửa downstream object. Nó phải phát hiện, mark stale và yêu cầu disposition/action theo policy.

## 13. Integration & Sync

### 13.1 IntegrationConnection

```text
IntegrationConnection
- IntegrationConnectionId
- ProjectId
- IntegrationType
- Provider
- ExternalResourceId
- Status
- Configuration
```

Ví dụ GitHub repository binding hoặc external CI connection.

### 13.2 ExportSnapshot

```text
ExportSnapshot
- ExportSnapshotId
- ProjectId
- ProjectBaselineId?
- SchemaVersion
- GeneratedAt
- GeneratedByPrincipalId
- ManifestChecksum
- Status
```

### 13.3 ExportSnapshotItem

```text
ExportSnapshotItem
- ExportSnapshotId
- ObjectType
- ObjectId
- ObjectVersionId
- ExportPath
- Checksum
```

### 13.4 SyncProposal

Two-way sync không sửa canonical data trực tiếp. External changes phải tạo proposal/change set.

```text
SyncProposal
- SyncProposalId
- ProjectId
- IntegrationConnectionId
- SourceSnapshotId?
- Status
- DetectedAt
- SubmittedByPrincipalId
```

Sync proposal có thể sinh `ChangeRequest` sau validation/conflict analysis.

## 14. Machine Authentication

### 14.1 MachineCredential

```text
MachineCredential
- MachineCredentialId
- PrincipalId
- CredentialType
- SecretHash / PublicKeyReference
- ExpiresAt?
- RevokedAt?
- CreatedAt
```

Plaintext API token chỉ được trả về lúc tạo và không lưu lại dạng có thể đọc.

### 14.2 CredentialScope

```text
CredentialScope
- MachineCredentialId
- ProjectId
- Scope
```

Scopes ban đầu:

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

### 14.3 APICommandLog

```text
APICommandLog
- CommandId
- ProjectId
- PrincipalId
- CommandType
- IdempotencyKey?
- TargetObjectType?
- TargetObjectId?
- Result
- CreatedAt
```

External agent/service actions phải auditable và hỗ trợ idempotency khi cần.

## 15. Audit

### 15.1 AuditEvent

```text
AuditEvent
- AuditEventId
- ProjectId
- ActorPrincipalId
- EventType
- ObjectType
- ObjectId
- BeforeVersion?
- AfterVersion?
- Metadata
- CreatedAt
```

Các event tối thiểu cần audit:

- role/permission change;
- baseline;
- status transition;
- relation change;
- machine credential lifecycle;
- external command;
- change request decision;
- sync/import application.

## 16. Identity và key strategy

Mỗi traceable object có:

```text
Technical ID: immutable UUID/ULID
Human key: project-scoped stable key
```

Ví dụ:

```text
ProjectId = 01K...
Project.Key = ERP-LAB
RequirementId = 01K...
Requirement.Key = REQ-P2P-012
```

Human key có thể theo rule từ ProjectTemplate nhưng không dùng làm database primary key.

## 17. Source-of-truth policy

Canonical data cho governance nằm trong SaaS database.

```text
SaaS Canonical Model
   ↓ export/project snapshot
Repository / Local Folder
   ↓ optional detected changes
Sync Proposal
   ↓ review/conflict resolution
Change Request
   ↓ approved application
New SaaS Versions/Baseline
```

Không thiết kế flow `file local sửa → overwrite database` trực tiếp.

## 18. Conceptual ER view

```text
User ─────── Principal(Human)
                  │
                  ├── ProjectMembership ─── Project
                  │                             │
AI Principal ─────┘                             ├── Documents ── Versions
Service Principal ──────────────────────────────┤
                                                ├── Knowledge Objects ── Versions
                                                ├── Deliverables ── Versions
                                                ├── Tasks ── Inputs/Scopes/Results
                                                ├── Relations
                                                ├── Milestones
                                                ├── Verification Runs/Evidence
                                                ├── Change Requests/Impact Items
                                                ├── Integration Connections
                                                └── Baselines/Export Snapshots
```

## 19. Các invariant quan trọng

1. Object key unique trong project.
2. Object technical ID immutable.
3. Relation không được cross project nếu relation schema không cho phép.
4. Reverse relation không có editable source thứ hai.
5. Baseline version immutable.
6. Task transition đi qua domain command, không patch raw status.
7. Machine principal chỉ thực hiện action trong project/scope được cấp.
8. Deliverable Verified phải có successful verification trên target version/revision hợp lệ.
9. Sync conflict không được silent overwrite.
10. Baseline change phải tạo impact analysis khi relation policy yêu cầu.
11. Task Done và Deliverable Accepted là hai lifecycle độc lập.
12. Document version và semantic object version không bị đồng nhất.

## 20. Vị trí của AI trong model

AI là một `Principal(type=AIAgent)` và trở thành ProjectMember khi được add vào project.

```text
AIAgent Principal
   ↓ ProjectMembership + Role/Scopes
Task
   ↓ context API
External Agent Execution
   ↓ authenticated command API
TaskResult / Artifact / Verification / ChangeRequest
```

Không có `AITask`, không có `AIRequirement`, không có `AIProject`. Domain software project management phải hoạt động đầy đủ khi project không có AI member nào.
