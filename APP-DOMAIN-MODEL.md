# Software Project Governance SaaS — Domain Model

## 1. Mục tiêu

Domain model này là canonical conceptual model cho Software Project Governance SaaS. Hệ thống quản lý **project structure tree + traceability graph + version/change lifecycle** để user có thể tổ chức tài liệu, quản lý software-project entities và đánh giá impact khi project thay đổi.

Nguyên tắc:

1. Project là data/governance boundary.
2. Folder/document structure và semantic traceability là hai model khác nhau.
3. Document là authoring container và cũng có thể là traceable entity.
4. Knowledge object có identity/version độc lập với document version.
5. Deliverable là thứ project cần tồn tại; Task là work để tạo/thay đổi deliverable.
6. Relation là canonical typed edge giữa **generic traceable entities**, không chỉ giữa KnowledgeObjects.
7. Baseline/versioned change phải có lịch sử và impact handling.
8. Human, AI Agent và Service Account đều là Principals; AI không tạo domain model riêng.
9. External repository/folder là projection/integration, không phải canonical governance store song song.

`docs/sample-project/` chỉ là acceptance fixture để kiểm tra model có biểu diễn được một project-document structure phức tạp hay không.

## 2. Bounded Contexts

```text
Identity & Access
Project Governance
Template Management
Project Structure
Documents & Knowledge
Deliverables
Work & Planning
Traceability
Verification
Change & Impact
Integration & Sync
Audit
```

MVP có thể là modular monolith; bounded context phân responsibility, không bắt buộc microservice.

## 3. Identity & Access

### 3.1 User

```text
User
- UserId
- Email
- DisplayName
- Status
- CreatedAt
```

`User` là human SaaS account toàn cục.

### 3.2 Principal

```text
Principal
- PrincipalId
- PrincipalType: Human | AIAgent | Service
- UserId?                 # chỉ Human
- DisplayName
- Status
- Metadata
```

### 3.3 ProjectMembership

```text
ProjectMembership
- ProjectMembershipId
- ProjectId
- PrincipalId
- Status
- JoinedAt
```

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

Authorization được enforce backend-side theo ProjectMembership + Role/Permission/Scope.

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

### 4.2 ProjectBaseline

```text
ProjectBaseline
- ProjectBaselineId
- ProjectId
- Key
- Name
- Status
- CreatedByPrincipalId
- CreatedAt
```

### 4.3 ProjectBaselineItem

```text
ProjectBaselineItem
- ProjectBaselineId
- EntityType
- EntityId
- EntityVersionRef
```

`EntityVersionRef` là logical reference tới exact version/revision của entity. Physical DB design có thể map tới typed version tables.

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
- PolicyDefinition
- PublishedAt?
```

PolicyDefinition chứa ID rules, enabled entity types, relation vocabulary, validation policy, lifecycle, role presets, export layout và các rule khác.

### 5.3 ProjectStructureTemplateNode

Structure template phải là explicit tree, không chỉ là blob khó query.

```text
ProjectStructureTemplateNode
- TemplateNodeId
- ProjectTemplateVersionId
- ParentTemplateNodeId?
- NodeType: Folder | Document
- Name
- SortOrder
- DocumentTemplateVersionId?   # required khi NodeType=Document
- Required
- Metadata
```

Invariant: tree không cycle; Document node phải reference valid DocumentTemplateVersion.

### 5.4 DocumentTemplate

```text
DocumentTemplate
- DocumentTemplateId
- Key
- Name
- DocumentType
- Status
```

### 5.5 DocumentTemplateVersion

```text
DocumentTemplateVersion
- DocumentTemplateVersionId
- DocumentTemplateId
- Version
- SchemaVersion
- ContentTemplate
- PublishedAt?
```

### 5.6 TemplateSection

```text
TemplateSection
- TemplateSectionId
- DocumentTemplateVersionId
- ParentTemplateSectionId?
- SectionKey
- Title
- SortOrder
- Required
- AllowedPlacementTypes[]?
```

## 6. Project Structure

### 6.1 ProjectStructureNode

Entity này giải quyết CRUD folder/document tree.

```text
ProjectStructureNode
- StructureNodeId
- ProjectId
- ParentStructureNodeId?
- NodeType: Folder | Document
- Name
- SortOrder
- DocumentId?              # required khi NodeType=Document
- Status: Active | Archived
- CreatedAt
- UpdatedAt
```

Rules:

- parent phải cùng Project;
- tree không cycle;
- Folder không reference Document;
- Document node reference đúng một Document cùng Project;
- MVP: một Document có tối đa một primary Document structure node;
- move/rename/reorder node không thay Document identity;
- StructureNode không tự động là traceability endpoint.

`CanonicalPath` nên derive từ tree thay vì dùng làm identity.

## 7. Documents & Knowledge

### 7.1 Document

```text
Document
- DocumentId
- ProjectId
- Key
- Title
- DocumentType
- DocumentTemplateVersionId?
- Status
- CurrentVersionId
- OwnerMembershipId?
- CreatedAt
- UpdatedAt
```

Không đặt `ParentDocumentId` trong Document. Parent/navigation thuộc `ProjectStructureNode`.

Document là traceable entity và có stable ID/key riêng với path.

### 7.2 DocumentVersion

```text
DocumentVersion
- DocumentVersionId
- DocumentId
- VersionNumber
- Content
- Status
- CreatedByPrincipalId
- CreatedAt
- ChangeSummary
```

### 7.3 DocumentSection

```text
DocumentSection
- DocumentSectionId
- DocumentId
- TemplateSectionId?
- ParentDocumentSectionId?
- SectionKey
- Title
- SortOrder
```

### 7.4 KnowledgeObject

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

Initial types:

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

### 7.5 KnowledgeObjectVersion

```text
KnowledgeObjectVersion
- KnowledgeObjectVersionId
- KnowledgeObjectId
- VersionNumber
- Payload
- Status
- CreatedByPrincipalId
- CreatedAt
- BaselineAt?
```

### 7.6 KnowledgePlacement

```text
KnowledgePlacement
- PlacementId
- KnowledgeObjectId
- KnowledgeObjectVersionId?
- DocumentId
- DocumentSectionId?
- Anchor?
- DisplayMode
- SortOrder
```

Placement trả lời **object được render ở đâu**, không trả lời semantic dependency.

Một KnowledgeObject có thể có nhiều placements.

## 8. Traceable Object Reference

Không ép tất cả entity vào một God Object nhưng domain cần một generic reference để graph dùng chung:

```text
TraceableRef
- ProjectId
- EntityType
- EntityId
```

Traceability endpoint tối thiểu:

```text
Document
KnowledgeObject
Deliverable
Task
VerificationDefinition
Milestone
ImplementationArtifact
ChangeRequest
ProjectBaseline / ExportSnapshot khi policy yêu cầu
```

Physical database có thể implement bằng polymorphic `(EntityType, EntityId)` với application validation hoặc một lightweight traceable-entity registry. Quyết định physical schema thuộc detailed design; conceptual invariant là endpoint phải resolve về một entity cùng project.

## 9. Relation

### 9.1 Relation

```text
Relation
- RelationId
- ProjectId
- FromEntityType
- FromEntityId
- RelationType
- ToEntityType
- ToEntityId
- FromVersionRef?
- ToVersionRef?
- CreatedByPrincipalId
- CreatedAt
- Metadata
```

Relation là canonical directed edge. Reverse view được generated.

Optional version refs dùng khi relation cần pin exact semantic revision; không bắt buộc mọi relation phải pin version.

### 9.2 RelationTypeDefinition

```text
RelationTypeDefinition
- RelationType
- ReverseDisplayName
- AllowedFromTypes[]
- AllowedToTypes[]
- IsAcyclic
- IsVersionSensitive
- ImpactPropagationMode: None | Direct | Transitive | ReviewOnly
- ImpactDirection: Forward | Reverse | Both
- CardinalityPolicy?
- Description
```

### 9.3 Canonical vocabulary

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

Semantics chuẩn:

```text
Requirement --requires-----> Deliverable
DesignSpec  --specifies----> Deliverable
Task        --implements---> Deliverable
VerificationDefinition --verifies--> Requirement / Deliverable
Task/TaskResult --produces--> ImplementationArtifact
```

`contained-in` không dùng cho folder/document structure.

## 10. Deliverable Management

### 10.1 Deliverable

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

### 10.2 DeliverableVersion

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

Lifecycle:

```text
Planned → Specified → Implemented → Verified → Accepted → Deprecated
```

Deliverable relationship tới Requirement/Design/Task/Verification nằm ở Relation graph, không duplicate thành editable arrays trên Deliverable.

## 11. Work & Planning

### 11.1 Roadmap / Phase / Milestone

```text
Roadmap
- RoadmapId
- ProjectId
- Key
- Name
- Status

Phase
- PhaseId
- RoadmapId
- Key
- Name
- SortOrder
- Status

Milestone
- MilestoneId
- ProjectId
- PhaseId?
- Key
- Name
- Status
- TargetDate?
```

### 11.2 MilestoneOutcome

```text
MilestoneOutcome
- MilestoneId
- DeliverableId
- RequiredState
```

Progress theo outcome/deliverable, không chỉ task count.

### 11.3 Task

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

Lifecycle:

```text
Draft → Ready → InProgress → Review → Done
          ↕          ↕
        Blocked    Blocked
```

`Cancelled` terminal riêng.

### 11.4 TaskInput

```text
TaskInput
- TaskId
- EntityType
- EntityId
- RequiredVersionRef?
- Required
```

### 11.5 TaskScope

```text
TaskScope
- TaskId
- TargetEntityType
- TargetEntityId
- Action: Create | Modify | Remove | Verify
```

### 11.6 TaskDependency

Task-to-task execution dependency có thể lưu explicit entity để planning hiệu quả:

```text
TaskDependency
- FromTaskId            # consumer/dependent
- ToTaskId              # prerequisite
- DependencyType
```

Nó phải consistent với generic `depends-on` semantics. Application service không được tạo hai nguồn sự thật mâu thuẫn; TaskDependency có thể là optimized projection của canonical dependency relation hoặc canonical specialized relation được exposed qua graph.

### 11.7 TaskResult

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

## 12. Implementation Artifact

```text
ImplementationArtifact
- ArtifactId
- ProjectId
- Key?
- ArtifactType
- ExternalSystem
- Repository?
- PathOrExternalId
- Revision
- Checksum?
- Metadata
- CreatedAt
```

Types:

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

Deliverable khác ImplementationArtifact: API là project deliverable; controller/OpenAPI/PR là artifacts hiện thực nó.

## 13. Verification

### 13.1 VerificationDefinition

```text
VerificationDefinition
- VerificationDefinitionId
- ProjectId
- Key
- Type
- Name
- Definition
- Status
```

### 13.2 VerificationRun

```text
VerificationRun
- VerificationRunId
- VerificationDefinitionId
- TargetEntityType
- TargetEntityId
- TargetVersionRef?
- ImplementationRevision?
- ExecutedByPrincipalId?
- ExecutedAt
- Status
- ExternalRunId?
```

### 13.3 Evidence

```text
Evidence
- EvidenceId
- VerificationRunId
- EvidenceType
- UriOrPayload
- Checksum?
```

VerificationDefinition là traceability endpoint; VerificationRun/Evidence là execution history/evidence.

## 14. Change & Impact

### 14.1 ChangeRequest

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

### 14.2 ChangeItem

```text
ChangeItem
- ChangeItemId
- ChangeRequestId
- TargetEntityType
- TargetEntityId
- FromVersionRef?
- ProposedPayloadOrPatch
- ChangeType
```

### 14.3 ImpactItem

```text
ImpactItem
- ImpactItemId
- ChangeRequestId
- EntityType
- EntityId
- CurrentVersionRef?
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

Impact traversal dùng generic Relation graph. Structural tree move/rename không tự propagate semantic impact.

## 15. Staleness

Version-sensitive relation/input phải có khả năng so sánh version ref đã pin với current/baseline version.

Possible states:

```text
Current
StaleReview
StaleRevalidate
Superseded
```

Stale không đồng nghĩa invalid; disposition/policy quyết định action.

## 16. Integration & Sync

### 16.1 IntegrationConnection

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

### 16.2 ExportSnapshot

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

### 16.3 ExportSnapshotItem

```text
ExportSnapshotItem
- ExportSnapshotId
- EntityType
- EntityId
- EntityVersionRef?
- StructureNodeId?
- ExportPath
- Checksum
```

Snapshot phải preserve cả structure projection và traceable identity/version.

### 16.4 SyncProposal

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

External changes không apply canonical state trực tiếp.

## 17. Machine Authentication

```text
MachineCredential
- MachineCredentialId
- PrincipalId
- CredentialType
- SecretHash / PublicKeyReference
- ExpiresAt?
- RevokedAt?
- CreatedAt

CredentialScope
- MachineCredentialId
- ProjectId
- Scope
```

External commands phải auditable, permission-checked, optimistic-concurrency-safe và idempotent khi có side effect.

## 18. Audit

```text
AuditEvent
- AuditEventId
- ProjectId
- ActorPrincipalId
- EventType
- EntityType
- EntityId
- BeforeVersionRef?
- AfterVersionRef?
- Metadata
- CreatedAt
```

Audit tối thiểu: permission, baseline, status transition, structure mutation, relation mutation, credential lifecycle, change decision, import/sync application.

## 19. Identity Strategy

Traceable entity dùng:

```text
Technical ID: immutable UUID/ULID
Human key: stable project-scoped key khi entity cần human reference
```

Path/title/name không phải identity.

Folder node có technical ID; human key không bắt buộc trong MVP vì folder chủ yếu là navigation entity.

## 20. Conceptual View

```text
Project
│
├── Structure Tree
│   └── ProjectStructureNode
│       ├── Folder
│       └── DocumentNode ──> Document ──> DocumentVersion
│                               │
│                               ├── DocumentSection
│                               └── KnowledgePlacement ──> KnowledgeObject ──> KO Version
│
├── Deliverable ──> DeliverableVersion
├── Task ──> Inputs / Scope / Result
├── VerificationDefinition ──> Run ──> Evidence
├── Milestone / Roadmap
├── ChangeRequest ──> ChangeItem / ImpactItem
└── Relation Graph
    TraceableRef ── Relation ── TraceableRef
```

## 21. Core Invariants

1. ProjectStructureNode tree không cycle.
2. Node parent/reference phải cùng Project.
3. Document path/name không phải identity; move/rename giữ DocumentId/Key.
4. DocumentVersion và KnowledgeObjectVersion độc lập.
5. KnowledgePlacement không được dùng thay semantic Relation.
6. Relation endpoint resolve được generic traceable entity cùng Project.
7. Relation source/target/type phải tuân RelationTypeDefinition.
8. Reverse relation không có editable duplicate.
9. Baseline version immutable.
10. Task lifecycle khác Deliverable lifecycle.
11. Task Ready đi qua domain policy; không patch raw status.
12. Deliverable Verified cần valid verification trên target version/revision.
13. Version-sensitive consumer phải có thể xác định staleness.
14. Structural-only change không tạo semantic impact mặc định.
15. Sync conflict không silent overwrite.
16. Machine principal chỉ act trong membership/scope.

## 22. AI trong model

AI là `Principal(type=AIAgent)`:

```text
AIAgent Principal
   ↓ ProjectMembership + Scopes
Task
   ↓ Context resolution
External execution
   ↓ API commands
TaskResult / Artifact / VerificationRun / ChangeRequest
```

Không có `AIProject`, `AITask` hay `AIRequirement`. Core domain phải hoạt động đầy đủ khi project không dùng AI.