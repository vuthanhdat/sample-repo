# External Integration, Synchronization và Change Management

## 1. Mục đích

Tài liệu này định nghĩa boundary giữa Software Project Governance SaaS và source repository, local folder, CI/CD, quality tools, CLI và AI/service clients.

Mục tiêu:

1. Export được **Project Structure Tree + traceable entities + relations + exact versions** ra external environment.
2. External actor cập nhật task/result/artifact/verification/change qua protocol an toàn.
3. Detect external changes mà không tạo source-of-truth cạnh tranh.
4. Quản lý baseline/change/impact/staleness một cách nhất quán với generic Traceability Graph.

`docs/sample-project/` chỉ là acceptance fixture; integration layer phải có khả năng export một project runtime thành structure tương tự, không dùng sample làm canonical data.

## 2. Source-of-truth boundary

```text
Governance / project semantic state  → SaaS
Source code / commits / PRs          → SCM
CI runs / logs / raw CI artifacts    → CI provider
External document source             → provider của document đó nếu configured
```

SaaS giữ stable references và metadata cần thiết; không copy mọi binary/log nếu external provider mới là canonical source.

## 3. Export model

User có thể export:

- whole project;
- ProjectBaseline;
- structure subtree;
- milestone scope;
- task context bundle;
- selected traceable entities.

Export phải preserve cả **where** và **why**:

```text
where → ProjectStructureNode tree / export path
why   → Traceable IDs, versions, relations, baseline
```

### 3.1 Standard bundle

```text
.project-governance/
  manifest.yaml
  project.yaml
  structure.yaml
  baseline.yaml
  relations.yaml
  documents/
    ... rendered folder/document tree ...
  objects/
    knowledge-objects.yaml
    deliverables.yaml
    tasks.yaml
    verification-definitions.yaml
```

`structure.yaml` là projection của `ProjectStructureNode`. `relations.yaml` là projection của canonical Relation graph. Hai file không được merge thành một model mơ hồ.

### 3.2 Manifest

Manifest tối thiểu:

```yaml
schemaVersion: "1.0"
project:
  id: "01K..."
  key: "ERP-LAB"
export:
  id: "EXP-01K..."
  generatedAt: "2026-09-22T09:30:00+07:00"
  baseline: "BL-2026-09-001"
  mode: "full"
entities:
  - key: "REQ-P2P-012"
    type: "KnowledgeObject.Requirement"
    version: 4
  - key: "DOC-P2P-REQ-001"
    type: "Document"
    version: 3
structure:
  rootChecksum: "..."
relationsChecksum: "..."
```

### 3.3 Document front matter

Rendered Markdown có thể có:

```yaml
---
projectKey: ERP-LAB
documentKey: DOC-P2P-REQ-001
documentId: 01K...
documentVersion: 3
structureNodeId: 01K...
baseline: BL-2026-09-001
exportId: EXP-01K...
---
```

Semantic objects rendered bên trong document vẫn giữ own object IDs/versions; không đồng nhất document version với semantic version.

## 4. Repository binding

Project có thể bind nhiều repositories:

```text
Project
├── RepositoryConnection(frontend)
├── RepositoryConnection(backend)
└── RepositoryConnection(infra)
```

Connection metadata:

- provider;
- external repository ID;
- default branch;
- governance export path;
- webhook configuration;
- sync mode;
- credential reference.

Initial modes:

```text
MANUAL_EXPORT
PUSH_EXPORT
PR_EXPORT
DETECT_EXTERNAL_CHANGES
TWO_WAY_REVIEWED_SYNC
```

MVP ưu tiên MANUAL_EXPORT/PUSH_EXPORT. Reviewed two-way sync chỉ triển khai khi identity/version/change model ổn định.

## 5. API model

### 5.1 Query APIs

Tối thiểu:

```text
GET /api/v1/projects/{projectKey}
GET /api/v1/projects/{projectKey}/structure
GET /api/v1/projects/{projectKey}/structure/{nodeId}
GET /api/v1/projects/{projectKey}/documents/{documentKey}
GET /api/v1/projects/{projectKey}/objects/{objectKey}
GET /api/v1/projects/{projectKey}/deliverables/{deliverableKey}
GET /api/v1/projects/{projectKey}/tasks/{taskKey}
GET /api/v1/projects/{projectKey}/tasks/{taskKey}/context
GET /api/v1/projects/{projectKey}/relations
GET /api/v1/projects/{projectKey}/backlinks?entity={key}
GET /api/v1/projects/{projectKey}/impact?root={key}
```

### 5.2 Command APIs

Lifecycle-sensitive updates dùng command semantics:

```text
POST /tasks/{taskKey}/transitions
POST /tasks/{taskKey}/results
POST /artifacts
POST /verification-runs
POST /change-requests
POST /impact-items/{id}/disposition
POST /sync-proposals
```

Structure/document CRUD vẫn có REST/application commands tương ứng nhưng phải dùng optimistic concurrency cho update nhạy cảm.

### 5.3 Optimistic concurrency

External update gửi expected version/ETag.

```json
{
  "transition": "start",
  "expectedVersion": 7
}
```

Version mismatch trả conflict, không overwrite.

### 5.4 Idempotency

External command có side effect hỗ trợ `Idempotency-Key` để retry an toàn.

## 6. Machine identity

AI Agent/CLI automation/CI integration dùng machine principal riêng:

```text
Principal(type=AIAgent|Service)
   ↓
ProjectMembership
   ↓
Role/Permission + CredentialScope
```

MVP API token:

- high entropy;
- plaintext shown once;
- hash stored server-side;
- revoke/rotate;
- optional expiry;
- project scoped;
- least privilege;
- full audit actor.

AI/service mặc định không có quyền project admin, member management, baseline approval hay deliverable acceptance.

## 7. Task execution protocol

External worker flow:

```text
1. Authenticate
2. GET task/{key}/context
3. Validate Ready + expected task/baseline version
4. POST transition=start
5. Execute in repository/tooling
6. POST implementation artifacts / result
7. Run/submit verification where applicable
8. POST transition=submit-for-review
```

Context bundle phải include:

- Task + task version;
- exact TaskInput versions;
- related Documents;
- target Deliverables;
- required BusinessRules/AcceptanceCriteria/DesignSpecifications;
- allowed scope;
- dependencies;
- verification requirements;
- current ProjectBaseline;
- relevant repository connections.

Context bundle là projection; canonical source vẫn là SaaS entities/relations.

## 8. External artifact ingestion

Example:

```text
TASK-P2P-BE-042 --produces--> PR-381
TASK-P2P-BE-042 --produces--> COMMIT-abc123
```

Artifact stores provider/resource/revision metadata. Full file content không bắt buộc nếu SCM là source of truth.

Webhook ingestion có thể nhận:

- PR opened/merged;
- commit pushed;
- workflow completed;
- test report available;
- deployment completed.

Mapping về Task/Deliverable phải dựa trên explicit metadata/reference khi có thể; không dựa hoàn toàn vào NLP commit message.

## 9. Canonical relation directions in integration data

Export/import/API phải dùng cùng vocabulary với product model:

```text
Requirement --requires-------> Deliverable
Requirement --satisfied-by---> DesignSpecification
DesignSpec  --specifies------> Deliverable
Task        --implements-----> Deliverable
Task/Result --produces-------> ImplementationArtifact
VerificationDefinition --verifies--> Requirement / Deliverable
```

Không export canonical edge đảo chiều chỉ vì UI thích label `specified-by` hay `implemented-by`. Reverse labels là view concern.

## 10. Change triggers

ChangeRequest có thể được trigger bởi:

```text
Human proposal
Requirement edit
Design review
Relation change
Deliverable contract change
External sync proposal
Production defect
Security finding
Dependency/platform upgrade
Verification failure
AI discovered issue
```

Structural tree move/rename/reorder **không mặc định** là semantic change trigger.

## 11. Change lifecycle

```text
Draft
  ↓
ImpactAnalysis
  ↓
Review
  ├── Rejected
  ↓
Approved
  ↓
Applying
  ↓
Verified
  ↓
Closed
```

Approved change phải giữ audit về source, changed entities, exact old/new versions và downstream dispositions.

## 12. Impact engine

### 12.1 Graph traversal

Impact engine traverse generic Relation graph theo `RelationTypeDefinition`.

Correct example:

```text
REQ-P2P-012 v4 → v5
   │
   ├── satisfied-by → API-P2P-007-SPEC
   │                     │
   │                     └── specifies → API-P2P-007
   │                                         ↑
   │                                         └── implements ← TASK-P2P-BE-042
   │
   └── verified-by (reverse view) ← VER-P2P-012
```

Canonical stored edges tương ứng:

```text
REQ-P2P-012 --satisfied-by--> API-P2P-007-SPEC
API-P2P-007-SPEC --specifies--> API-P2P-007
TASK-P2P-BE-042 --implements--> API-P2P-007
VER-P2P-012 --verifies-------> REQ-P2P-012
```

Traversal engine có thể đi forward/reverse theo impact policy; canonical relation direction không cần đổi.

### 12.2 Impact policy

RelationTypeDefinition tối thiểu có:

```text
ImpactPropagationMode: None | Direct | Transitive | ReviewOnly
ImpactDirection: Forward | Reverse | Both
IsVersionSensitive
```

Không phải mọi edge propagate giống nhau.

### 12.3 Potential impact, không phải automatic change

Graph discovery tạo `ImpactItem`. Nó không tự quyết định downstream phải sửa.

Disposition:

```text
UpdateRequired
ReviewRequired
RevalidationRequired
ReplanRequired
NoChangeRequired
Obsolete
```

Ví dụ Requirement wording thay đổi nhưng API contract vẫn đúng:

```text
API DesignSpec   → ReviewRequired
API Deliverable  → NoChangeRequired
Verification     → RevalidationRequired
```

## 13. Structural change handling

Tách rõ:

```text
Structure change
- rename folder
- move document
- reorder siblings

Semantic change
- document content/version changes with governed meaning
- requirement/design/deliverable version changes
- relation add/remove/change
- policy change
```

Structure-only change mặc định chỉ tạo audit + export path change. Nó không mark related Requirement/Task stale.

Nếu ProjectTemplate định nghĩa path là external contract, change đó có thể trigger explicit path-sensitive impact policy.

## 14. Change application

Trước apply phải có Change Plan, ví dụ:

```text
CR-2026-004
├── UPDATE REQ-P2P-012 v4 → v5
├── UPDATE API-P2P-007-SPEC v2 → v3
├── UPDATE API-P2P-007 v2 → v3
├── CREATE TASK-P2P-BE-081
├── REVALIDATE VER-P2P-012
└── MARK EXP-103 stale
```

Apply không xóa old versions.

## 15. Staleness

Version-sensitive consumer record exact version/baseline:

```text
TASK-P2P-BE-042 inputs:
- REQ-P2P-012@v4
- API-P2P-007-SPEC@v2
```

Khi upstream baseline đổi:

```text
Current
StaleReview
StaleRevalidate
Superseded
```

Stale không tự đồng nghĩa invalid; policy/disposition quyết định action.

## 16. Two-way sync protocol

External edits không apply thẳng:

```text
External files
   ↓ parse identity/export metadata
SyncProposal
   ↓ diff against source ExportSnapshot
Detected Changes
   ↓ conflict detection
ChangeRequest / Import Review
   ↓ approval
Apply canonical versions/structure/relations
```

Conflict classes:

```text
NO_CONFLICT
APP_CHANGED_ONLY
EXTERNAL_CHANGED_ONLY
BOTH_CHANGED
UNKNOWN_BASELINE
IDENTITY_CONFLICT
SCHEMA_CONFLICT
STRUCTURE_CONFLICT
RELATION_CONFLICT
```

`BOTH_CHANGED`, `IDENTITY_CONFLICT`, `RELATION_CONFLICT` không auto-merge nếu semantic safety không rõ.

## 17. Identity rules for import

Import resolve theo stable metadata, không filename/title đơn thuần:

- ProjectId/ProjectKey;
- StructureNodeId khi xử lý tree;
- EntityId/Key;
- exact source ExportSnapshot;
- version/revision;
- checksums.

Rename/move file ngoài repository có thể map về structure mutation nếu identity metadata vẫn còn. Nếu mất identity, tạo conflict/proposal thay vì đoán.

## 18. CLI direction

Future CLI:

```text
projgov project pull
projgov project export
projgov structure tree
projgov entity get REQ-P2P-012
projgov relations REQ-P2P-012
projgov impact REQ-P2P-012
projgov task context TASK-P2P-BE-042
projgov task start TASK-P2P-BE-042
projgov task submit TASK-P2P-BE-042 --result result.json
projgov sync propose
```

CLI là API client, không chứa business rules độc lập.

## 19. Events / Outbox

Potential outbound events:

```text
StructureChanged
DocumentVersionCreated
RelationChanged
TaskReady
TaskStatusChanged
RequirementBaselined
DeliverableVersionChanged
ChangeRequestApproved
ImpactDetected
VerificationFailed
BaselineCreated
ExportSnapshotCreated
```

External delivery dùng retry/outbox; external endpoint outage không làm core transaction thất bại.

## 20. Security requirements

- project boundary enforced everywhere;
- external token scoped/revocable;
- credential secret protected;
- webhook authenticity validated;
- external commands audited;
- optimistic concurrency on sensitive updates;
- idempotency for retried commands;
- import never bypasses authorization/lifecycle rules;
- relation endpoint ownership/project validation mandatory;
- export must not leak entities outside selected project/scope.

## 21. Acceptance scenarios

Integration/change layer phải hỗ trợ:

1. Export project structure tương đương `docs/sample-project/` cùng stable document IDs.
2. Export relations tách khỏi folder structure.
3. Move document rồi export lại: path đổi nhưng DocumentId/Key không đổi.
4. Requirement version change tạo potential impact qua DesignSpec/Deliverable/Task/Verification.
5. Reverse UI path vẫn query được từ canonical relations, không cần duplicate reverse edges.
6. External edit từ stale snapshot tạo conflict/proposal, không overwrite canonical state.
7. Task context bundle resolve exact versions mà external AI/human cần đọc.

> **Integration không được phá ownership của dữ liệu: SaaS quản lý governance state, SCM quản lý source code, CI quản lý execution result; mọi bridge giữa chúng phải giữ identity, version, audit và change semantics.**