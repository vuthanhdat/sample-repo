# Software Project Governance SaaS — Domain Model

## 1. Purpose

Domain model này là canonical conceptual model cho sản phẩm được định nghĩa tại [PRODUCT-AREAS.md](./PRODUCT-AREAS.md).

Model phải hỗ trợ ba Product Area:

```text
PA-01 Template & Methodology Management
        ↓ instantiate
PA-02 Project Workspace & Traceability
        ↓ execute / collaborate
PA-03 Work Management & Human-AI Collaboration
```

Nguyên tắc nền tảng:

1. Template là methodology/metamodel; Project là runtime instance.
2. Template object và runtime object có identity/lifecycle riêng.
3. Project Structure Tree và Traceability Graph là hai model khác nhau.
4. Requirement/Design/Deliverable/Task/Verification phải trace được bằng stable IDs và typed relations.
5. Task template/tasklist template là first-class domain của PA-01, không chỉ có runtime Task.
6. Coverage/completeness policy là first-class methodology data, không hard-code hoàn toàn trong UI.
7. Human, AI Agent và Service đều là Principals; AI không có project/task schema riêng.
8. `Task Done` không đồng nghĩa `Deliverable Accepted` hoặc `Requirement Complete`.
9. Baseline/versioned change phải có history, staleness và impact handling.
10. External repository là projection/integration target, không phải canonical governance store song song.

`docs/sample-project/` chỉ là acceptance fixture.

---

# 2. Product Area → Domain mapping

```text
PA-01 Template & Methodology
├── Project Template
├── Structure Template
├── Document Template
├── Semantic Schema Template
├── Relation Policy Template
├── Deliverable Policy Template
├── Task Template
├── Tasklist Template
└── Governance / Coverage Policy

PA-02 Project Workspace & Traceability
├── Project
├── Project Structure
├── Documents / Knowledge
├── Requirements / Designs
├── Relations / Traceability
├── Deliverables
├── Runtime Tasks / Planning
├── Verification
└── Baseline / Change / Impact

PA-03 Work Management & Human-AI Collaboration
├── Assignment / Work Queue
├── Task Context Resolution
├── Execution Result / Artifact
├── Blocker / Review / Handoff
└── Dashboard Projections

Platform / Cross-cutting
├── Identity & Access
├── Versioning / Audit
├── Integration & Sync
└── Search / API
```

MVP có thể implement bằng modular monolith. Product Area phân business capability; module/bounded context phân code responsibility. Hai khái niệm không bắt buộc 1:1.

---

# 3. Platform Identity & Access

## 3.1 User

```text
User
- UserId
- Email
- DisplayName
- Status
- CreatedAt
```

`User` là human SaaS account toàn cục.

## 3.2 Principal

```text
Principal
- PrincipalId
- PrincipalType: Human | AIAgent | Service
- UserId?                 # Human only
- DisplayName
- Status
- Metadata
```

## 3.3 ProjectMembership

```text
ProjectMembership
- ProjectMembershipId
- ProjectId
- PrincipalId
- Status
- JoinedAt
```

## 3.4 Role / Permission

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

---

# 4. PA-01 — Template & Methodology Domain

## 4.1 ProjectTemplate

```text
ProjectTemplate
- ProjectTemplateId
- Key
- Name
- Description
- Status
- OwnerPrincipalId
- CreatedAt
```

## 4.2 ProjectTemplateVersion

```text
ProjectTemplateVersion
- ProjectTemplateVersionId
- ProjectTemplateId
- Version
- SchemaVersion
- Status: Draft | Published | Deprecated
- CreatedByPrincipalId
- CreatedAt
- PublishedAt?
```

Published version immutable.

Một ProjectTemplateVersion aggregate references structure, document, semantic, relation, deliverable, tasklist và governance definitions tạo nên một methodology version hoàn chỉnh.

---

## 4.3 ProjectStructureTemplateNode

```text
ProjectStructureTemplateNode
- TemplateNodeId
- ProjectTemplateVersionId
- ParentTemplateNodeId?
- NodeType: Folder | Document
- Name
- SortOrder
- DocumentTemplateVersionId?   # required for Document node
- Required
- Metadata
```

Rules:

- tree không cycle;
- parent cùng ProjectTemplateVersion;
- Folder không reference DocumentTemplateVersion;
- Document node reference exact DocumentTemplateVersion.

---

## 4.4 DocumentTemplate

```text
DocumentTemplate
- DocumentTemplateId
- Key
- Name
- DocumentType
- Status
```

## 4.5 DocumentTemplateVersion

```text
DocumentTemplateVersion
- DocumentTemplateVersionId
- DocumentTemplateId
- Version
- SchemaVersion
- ContentTemplate
- Status: Draft | Published | Deprecated
- PublishedAt?
```

## 4.6 TemplateSection

```text
TemplateSection
- TemplateSectionId
- DocumentTemplateVersionId
- ParentTemplateSectionId?
- SectionKey
- Title
- SortOrder
- Required
- InitialContent?
- AllowedPlacementTypes[]?
- ValidationMetadata?
```

---

## 4.7 SemanticObjectTypeDefinition

Template phải định nghĩa semantic types được enable thay vì hard-code mọi project giống nhau.

```text
SemanticObjectTypeDefinition
- ObjectTypeDefinitionId
- ProjectTemplateVersionId
- ObjectType
- DisplayName
- SchemaDefinition
- KeyPattern?
- Versioned
- RequiredMetadata?
- Enabled
```

Initial object types có thể gồm:

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

---

## 4.8 RelationTypeDefinition

```text
RelationTypeDefinition
- RelationTypeDefinitionId
- ProjectTemplateVersionId
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

Canonical initial vocabulary:

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

Reverse view được generated; không lưu editable duplicate edge.

---

## 4.9 DeliverableTypePolicy

```text
DeliverableTypePolicy
- DeliverableTypePolicyId
- ProjectTemplateVersionId
- DeliverableType
- Enabled
- LifecyclePolicy
- RequiredDesignTypes[]
- RequiredVerificationTypes[]
- KeyPattern?
```

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

Example: API có thể yêu cầu API Contract, Authorization Design, Validation Design, Error Handling Design và Observability Design trước khi đạt `Specified`.

---

## 4.10 TaskTemplate

`TaskTemplate` mô tả một work blueprint, không phải runtime Task.

```text
TaskTemplate
- TaskTemplateId
- ProjectTemplateVersionId
- Key
- Name
- TaskType
- ObjectiveTemplate
- TargetEntityType?
- DefaultAssigneeRole?
- PriorityDefault?
- DefinitionOfReadyRuleSetId?
- DefinitionOfDoneRuleSetId?
- Status
```

## 4.11 TaskTemplateInputRule

```text
TaskTemplateInputRule
- TaskTemplateInputRuleId
- TaskTemplateId
- InputType
- SelectorExpression
- Required
- PinVersionPolicy
- Purpose
```

Input selector phải dựa trên semantic relation/type/key policy thay vì hard-code file path khi có thể.

## 4.12 TaskTemplateScopeRule

```text
TaskTemplateScopeRule
- TaskTemplateScopeRuleId
- TaskTemplateId
- TargetEntityType
- Action: Create | Modify | Remove | Verify
- SelectorExpression?
- Required
```

## 4.13 TaskTemplateVerificationRule

```text
TaskTemplateVerificationRule
- TaskTemplateVerificationRuleId
- TaskTemplateId
- VerificationType
- Required
- TargetSelector
```

---

## 4.14 TasklistTemplate

```text
TasklistTemplate
- TasklistTemplateId
- ProjectTemplateVersionId
- Key
- Name
- AppliesToEntityType?
- AppliesToSubtype?
- Description
- Status
```

Ví dụ `API Implementation Tasklist` áp dụng cho DeliverableType=API.

## 4.15 TasklistTemplateItem

```text
TasklistTemplateItem
- TasklistTemplateItemId
- TasklistTemplateId
- TaskTemplateId
- LocalKey
- SortOrder
- Required
- ConditionExpression?
```

## 4.16 TasklistTemplateDependency

```text
TasklistTemplateDependency
- TasklistTemplateId
- FromItemId          # dependent/consumer
- ToItemId            # prerequisite
- DependencyType
```

Dependency graph không cycle.

Khi instantiate vào project:

```text
TaskTemplate             → Task
TasklistTemplateItem     → Task instance
TasklistTemplateDependency → TaskDependency
```

Runtime Task không reuse template identity.

---

## 4.17 GovernanceRuleSet

```text
GovernanceRuleSet
- GovernanceRuleSetId
- ProjectTemplateVersionId
- Key
- Name
- RuleSetType:
    RequirementCompleteness
  | DeliverableReadiness
  | TaskDefinitionOfReady
  | TaskDefinitionOfDone
  | VerificationCompleteness
  | BaselinePolicy
- Status
```

## 4.18 GovernanceRule

```text
GovernanceRule
- GovernanceRuleId
- GovernanceRuleSetId
- RuleKey
- RuleType
- Expression / StructuredDefinition
- Severity: Error | Warning | Info
- MessageTemplate
- SortOrder
```

Governance rule trả lời các câu hỏi như:

```text
Requirement đã có AcceptanceCriterion chưa?
Requirement đã có đủ required DesignSpecification chưa?
Deliverable đã có đủ design products chưa?
Task đã có đủ context để Ready chưa?
Requirement đã có current verification chưa?
```

Rule engine physical implementation thuộc detailed design; conceptual model yêu cầu rule là versioned methodology data và explainable được reason khi fail.

---

# 5. PA-02 — Project Runtime Domain

## 5.1 Project

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

`ProjectTemplateVersionId` ghi provenance tới exact methodology source.

Template update không silently rewrite project runtime state.

---

## 5.2 ProjectStructureNode

```text
ProjectStructureNode
- StructureNodeId
- ProjectId
- ParentStructureNodeId?
- NodeType: Folder | Document
- Name
- SortOrder
- DocumentId?              # required for Document node
- SourceTemplateNodeId?
- Status: Active | Archived
- CreatedAt
- UpdatedAt
```

Rules:

- parent cùng Project;
- tree không cycle;
- Folder không reference Document;
- Document node reference đúng một Document cùng Project;
- MVP: một Document có tối đa một primary structure node;
- move/rename/reorder không thay Document identity;
- `CanonicalPath` derive từ tree;
- StructureNode không tự động là traceability endpoint.

---

# 6. Documents & Knowledge

## 6.1 Document

```text
Document
- DocumentId
- ProjectId
- Key
- Title
- DocumentType
- SourceDocumentTemplateVersionId?
- Status
- CurrentVersionId
- OwnerMembershipId?
- CreatedAt
- UpdatedAt
```

Không có `ParentDocumentId`; navigation parent thuộc ProjectStructureNode.

## 6.2 DocumentVersion

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

## 6.3 DocumentSection

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

## 6.4 KnowledgeObject

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

## 6.5 KnowledgeObjectVersion

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

## 6.6 KnowledgePlacement

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

---

# 7. Traceability Graph

## 7.1 TraceableRef

Conceptual generic reference:

```text
TraceableRef
- ProjectId
- EntityType
- EntityId
```

Traceability endpoints tối thiểu:

```text
Document
KnowledgeObject
Deliverable
Task
VerificationDefinition
Milestone
ImplementationArtifact
ChangeRequest
ProjectBaseline / ExportSnapshot when needed
```

Physical DB có thể dùng polymorphic `(EntityType, EntityId)` hoặc lightweight traceable registry. Detailed design quyết định implementation.

## 7.2 Relation

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

Relation là canonical directed edge.

Canonical semantics:

```text
Requirement --requires--------> Deliverable
Requirement --satisfied-by----> DesignSpecification
Requirement --accepted-by-----> AcceptanceCriterion
Requirement --governed-by-----> BusinessRule / Policy
DesignSpecification --specifies--> Deliverable
Task --implements-------------> Deliverable
VerificationDefinition --verifies--> Requirement / Deliverable
Task/TaskResult --produces----> ImplementationArtifact
```

`contained-in` không dùng cho folder/document structure.

---

# 8. Deliverable Management

## 8.1 Deliverable

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
- SourcePolicyRef?
```

## 8.2 DeliverableVersion

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

Transitions được validate bằng project methodology/governance policy.

---

# 9. Runtime Planning & Tasks

## 9.1 Roadmap / Phase / Milestone

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

## 9.2 MilestoneOutcome

```text
MilestoneOutcome
- MilestoneId
- DeliverableId
- RequiredState
```

Progress theo outcome/deliverable, không chỉ task count.

## 9.3 Task

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
- SourceTaskTemplateId?
- SourceTasklistTemplateId?
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

`Cancelled` là terminal state riêng.

## 9.4 TaskInput

```text
TaskInput
- TaskId
- EntityType
- EntityId
- RequiredVersionRef?
- Required
- Purpose?
- SourceTemplateInputRuleId?
```

## 9.5 TaskScope

```text
TaskScope
- TaskId
- TargetEntityType
- TargetEntityId
- Action: Create | Modify | Remove | Verify
- SourceTemplateScopeRuleId?
```

## 9.6 TaskDependency

```text
TaskDependency
- FromTaskId            # consumer/dependent
- ToTaskId              # prerequisite
- DependencyType
- SourceTemplateDependencyId?
```

Application service không được duy trì hai nguồn sự thật mâu thuẫn giữa TaskDependency và generic `depends-on`. Một trong hai phải canonical và cái còn lại là projection/index.

## 9.7 TaskResult

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

---

# 10. Verification

## 10.1 VerificationDefinition

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

## 10.2 VerificationRun

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

## 10.3 Evidence

```text
Evidence
- EvidenceId
- VerificationRunId
- EvidenceType
- UriOrPayload
- Checksum?
```

VerificationDefinition là traceability endpoint; VerificationRun/Evidence là execution history.

---

# 11. Coverage & Completeness

Coverage là **derived evaluation**, không nên lưu một percentage canonical duy nhất dễ stale.

Conceptual result:

```text
CoverageEvaluation
- ProjectId
- SubjectEntityType
- SubjectEntityId
- SubjectVersionRef?
- RuleSetId
- EvaluatedAt
- OverallState
- Score?
- Findings[]
```

```text
CoverageFinding
- RuleKey
- Status: Pass | Fail | Warning | NotApplicable
- MissingEntityType?
- MissingRelationType?
- RelatedEntityRef?
- Reason
```

Ví dụ Requirement coverage có thể evaluate:

```text
Requirement Defined
Acceptance Coverage
Design Coverage
Deliverable Coverage
Task Coverage
Implementation Status
Verification Currentness
```

Dashboard consume evaluation/query result; dashboard không phải source of truth.

---

# 12. Baseline, Change & Impact

## 12.1 ProjectBaseline

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

## 12.2 ProjectBaselineItem

```text
ProjectBaselineItem
- ProjectBaselineId
- EntityType
- EntityId
- EntityVersionRef
```

Baseline immutable.

## 12.3 ChangeRequest

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

## 12.4 ChangeItem

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

## 12.5 ImpactItem

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

Dispositions:

```text
UpdateRequired
ReviewRequired
RevalidationRequired
ReplanRequired
NoChangeRequired
Obsolete
```

Impact traversal dùng Relation graph. Structural-only move/rename không tự propagate semantic impact.

---

# 13. Staleness

Version-sensitive relation/input phải so sánh pinned version với current/baseline version.

Possible states:

```text
Current
StaleReview
StaleRevalidate
Superseded
```

Stale không đồng nghĩa invalid; policy/disposition quyết định action.

---

# 14. PA-03 — Execution & Collaboration Domain

PA-03 chủ yếu là application behavior/projection trên Task + Project Graph, nhưng một số execution facts cần canonical persistence.

## 14.1 TaskBlocker

```text
TaskBlocker
- TaskBlockerId
- TaskId
- BlockerType:
    MissingRequirement
  | AmbiguousRequirement
  | MissingDesign
  | MissingDependency
  | VerificationFailure
  | Permission
  | ExternalDependency
  | Other
- RelatedEntityType?
- RelatedEntityId?
- Description
- RaisedByPrincipalId
- RaisedAt
- ResolvedByPrincipalId?
- ResolvedAt?
- Resolution?
- Status: Open | Resolved
```

## 14.2 TaskReview

```text
TaskReview
- TaskReviewId
- TaskId
- TaskResultId?
- ReviewerMembershipId
- Status: Requested | InReview | Approved | ChangesRequested | Rejected
- Comment?
- CreatedAt
- ReviewedAt?
```

## 14.3 WorkHandoff

```text
WorkHandoff
- WorkHandoffId
- TaskId
- FromMembershipId?
- ToMembershipId?
- Reason
- CreatedByPrincipalId
- CreatedAt
```

Handoff giữ collaboration history nhưng Task.AssigneeMembershipId vẫn là current assignee source of truth.

## 14.4 TaskContextBundle

`TaskContextBundle` là **derived projection**, không phải duplicate canonical knowledge.

Conceptual output:

```text
TaskContextBundle
- TaskId / TaskVersion
- ProjectId / ProjectBaselineId?
- Objective
- InputRefs[]                # exact versions where required
- RequirementRefs[]
- AcceptanceCriterionRefs[]
- DesignRefs[]
- DeliverableRefs[]
- DocumentRefs[]
- DependencyRefs[]
- WriteScopes[]
- VerificationRequirements[]
- ArtifactRefs[]
- ResolutionReasons[]
```

Mỗi resolved item nên có path/reason giải thích vì sao nó thuộc context.

## 14.5 Dashboard projections

Các dashboard sau là query/read-model, không phải canonical aggregate mới:

```text
ProjectProgressProjection
RequirementCompletionProjection
TaskBoardProjection
BlockedWorkProjection
ReviewQueueProjection
TeamAgentActivityProjection
MilestoneProgressProjection
```

Chúng derive từ project entities, lifecycle states, coverage evaluation và audit/activity data.

---

# 15. Implementation Artifacts

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

---

# 16. Integration & Sync

## 16.1 IntegrationConnection

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

## 16.2 ExportSnapshot

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

## 16.3 ExportSnapshotItem

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

## 16.4 SyncProposal

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

External change không apply canonical state trực tiếp.

---

# 17. Machine Authentication

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

External commands phải permission-checked, auditable, optimistic-concurrency-safe và idempotent khi có side effect.

---

# 18. Audit

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

Audit tối thiểu:

- membership/permission;
- template publish;
- baseline;
- lifecycle transition;
- structure mutation;
- relation mutation;
- task assignment/result/review/blocker/handoff;
- credential lifecycle;
- change/impact decision;
- import/sync application.

---

# 19. Template → Runtime mapping

| PA-01 Template domain | PA-02 Runtime domain |
|---|---|
| `ProjectTemplateVersion` | `Project` + template provenance |
| `ProjectStructureTemplateNode` | `ProjectStructureNode` |
| `DocumentTemplateVersion` | `Document` + initial `DocumentVersion` |
| `TemplateSection` | `DocumentSection` |
| `SemanticObjectTypeDefinition` | `KnowledgeObject` instances |
| `RelationTypeDefinition` | validation policy for `Relation` instances |
| `DeliverableTypePolicy` | `Deliverable` lifecycle/coverage validation |
| `TaskTemplate` | `Task` |
| `TaskTemplateInputRule` | `TaskInput` |
| `TaskTemplateScopeRule` | `TaskScope` |
| `TasklistTemplateDependency` | `TaskDependency` |
| `GovernanceRuleSet` | `CoverageEvaluation` / transition validation |

Template object không giữ runtime status/progress.

---

# 20. Conceptual View

```text
PA-01 TEMPLATE

ProjectTemplateVersion
├── StructureTemplateNode ──> DocumentTemplateVersion ──> TemplateSection
├── SemanticObjectTypeDefinition
├── RelationTypeDefinition
├── DeliverableTypePolicy
├── TaskTemplate ──> InputRule / ScopeRule / VerificationRule
├── TasklistTemplate ──> Items / Dependencies
└── GovernanceRuleSet
          │
          │ instantiate
          ▼
PA-02 PROJECT RUNTIME

Project
├── Structure Tree ──> Document ──> DocumentVersion
│                         └── KnowledgePlacement ──> KnowledgeObject ──> Version
├── Deliverable ──> DeliverableVersion
├── Task ──> Input / Scope / Dependency / Result
├── VerificationDefinition ──> Run ──> Evidence
├── Baseline / Change / Impact
└── Relation Graph
      TraceableRef ── Relation ── TraceableRef
          │
          │ execute / query
          ▼
PA-03 COLLABORATION

TaskBoard / WorkQueue
TaskContextBundle
TaskBlocker / TaskReview / WorkHandoff
ProjectProgress / RequirementCompletion / TeamAgentActivity projections
```

---

# 21. Core invariants

1. Published ProjectTemplateVersion immutable.
2. Published DocumentTemplateVersion immutable.
3. TaskTemplate/TasklistTemplate không lưu runtime execution status.
4. TasklistTemplate dependency graph không cycle.
5. Project instance giữ provenance tới exact template version.
6. Template update không silently mutate existing project runtime state.
7. ProjectStructureNode tree không cycle.
8. Document path/name không phải identity; move/rename giữ DocumentId/Key.
9. DocumentVersion và KnowledgeObjectVersion độc lập.
10. KnowledgePlacement không thay semantic Relation.
11. Relation endpoint resolve được traceable entity cùng Project.
12. Relation source/target/type tuân RelationTypeDefinition/project policy.
13. Reverse relation không có editable duplicate.
14. Baseline version immutable.
15. Task lifecycle khác Deliverable lifecycle.
16. Task Ready phải pass configured DoR.
17. Task Done không suy ra Deliverable Accepted hay Requirement Complete.
18. Deliverable Verified cần valid verification trên target version/revision.
19. Coverage result phải explain được failed/missing rule, không chỉ trả percentage.
20. Version-sensitive consumer phải xác định được staleness.
21. Structural-only change không tạo semantic impact mặc định.
22. AI agent không được silently sửa canonical requirement khi thiếu/ambiguous context; phải raise blocker/proposal theo policy.
23. Dashboard là projection từ canonical project state, không là source of truth riêng.
24. Sync conflict không silent overwrite.
25. Machine principal chỉ act trong membership/scope.

---

# 22. AI in the model

AI là `Principal(type=AIAgent)`:

```text
AIAgent Principal
   ↓ ProjectMembership + Scope
Runtime Task
   ↓ deterministic TaskContextBundle
External execution
   ↓ API commands
TaskResult / Artifact / VerificationRun / Blocker / Review
   ↓
Coverage recalculation / project dashboard
```

Không có `AIProject`, `AITask` hay `AIRequirement`. Core domain phải hoạt động đầy đủ khi project không dùng AI.