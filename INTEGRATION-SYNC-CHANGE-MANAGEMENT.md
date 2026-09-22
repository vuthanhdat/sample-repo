# External Integration, Synchronization và Change Management

## 1. Mục đích

Tài liệu này định nghĩa boundary giữa Software Project Governance SaaS và các môi trường bên ngoài như source repository, local project folder, CI/CD, quality tools và AI agents. Mục tiêu là giữ SaaS làm canonical source of truth cho governance data nhưng vẫn cho phép developer/tool/AI làm việc trong môi trường quen thuộc và gửi kết quả trở lại app theo protocol có identity, version, audit và conflict control.

Ba vấn đề cần giải quyết:

1. Làm sao xuất project knowledge/task/output ra repository/folder để human và AI đọc được.
2. Làm sao external actor cập nhật task/result/artifact/evidence vào SaaS một cách an toàn.
3. Làm sao quản lý thay đổi để biết object/document/deliverable/task/verification nào bị ảnh hưởng.

## 2. System-of-record boundary

MVP sử dụng nguyên tắc:

```text
SaaS Database = canonical governance state
Repository/Folder = exported projection
External tools = producers/consumers through API
```

Điều này không có nghĩa source code nằm trong SaaS. Source repository vẫn là canonical source cho source code. Boundary chính xác là:

```text
Governance metadata / project semantic state → SaaS
Source code / commits / PRs                 → SCM (GitHub/GitLab/...)
CI run/log/test artifacts                   → CI provider
```

SaaS giữ reference tới external artifacts và ingestion metadata, không copy mọi binary/log nếu không cần thiết.

## 3. Export model

### 3.1 Export mục tiêu

User có thể export:

- toàn project;
- một baseline;
- một milestone;
- một task context bundle;
- một subtree/selection của documents/objects.

### 3.2 Standard bundle

Layout mặc định:

```text
.project-governance/
  manifest.yaml
  project.yaml
  baseline.yaml
  relations.yaml
  documents/
    00-governance/
    10-goals/
    20-requirements/
    30-design/
    40-deliverables/
    50-tasks/
    60-verification/
    70-changes/
  objects/
    goals.yaml
    requirements.yaml
    rules.yaml
    acceptance-criteria.yaml
    design-decisions.yaml
    deliverables.yaml
    tasks.yaml
```

Layout có thể được ProjectTemplate override, nhưng manifest schema phải ổn định/versioned.

### 3.3 Manifest

Ví dụ:

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
objects:
  - key: "REQ-P2P-012"
    type: "Requirement"
    version: 4
    path: "documents/20-requirements/REQ-P2P-012.md"
    checksum: "..."
```

Manifest giúp tool xác định identity/version thay vì suy luận từ filename.

### 3.4 Markdown front matter

Human-readable file có thể chứa:

```yaml
---
projectKey: ERP-LAB
objectKey: REQ-P2P-012
objectId: 01K...
objectType: Requirement
version: 4
baseline: BL-2026-09-001
exportId: EXP-01K...
---
```

Body có thể render canonical payload theo template.

### 3.5 Generated file policy

Exported files nên có marker cho biết phần nào generated và phần nào external-editable nếu mode đó được hỗ trợ. Không nên cho user sửa file generated rồi kỳ vọng app tự hiểu mọi thay đổi nếu chưa có import protocol.

## 4. Repository binding

### 4.1 RepositoryConnection

Project có thể bind một hoặc nhiều repository:

```text
Project
  ├── RepositoryConnection: frontend
  ├── RepositoryConnection: backend
  └── RepositoryConnection: infra
```

Connection metadata:

- provider;
- repository external ID;
- default branch;
- governance export path;
- webhook status;
- sync mode;
- credential reference.

### 4.2 Repository sync modes

Initial modes:

```text
MANUAL_EXPORT
PUSH_EXPORT
PR_EXPORT
DETECT_EXTERNAL_CHANGES
TWO_WAY_REVIEWED_SYNC
```

MVP nên bắt đầu `MANUAL_EXPORT` hoặc `PUSH_EXPORT`. `TWO_WAY_REVIEWED_SYNC` chỉ triển khai sau khi change/conflict model ổn định.

### 4.3 PR export

Một workflow an toàn hơn direct push:

```text
Create/Update SaaS Baseline
   ↓
Generate Export Snapshot
   ↓
Create Git branch / commit
   ↓
Open PR updating .project-governance/
   ↓
Normal repository review/merge
```

SaaS lưu mapping ExportSnapshot ↔ commit/PR.

## 5. External API model

### 5.1 Query APIs

External clients cần ít nhất:

```text
GET /api/v1/projects/{projectKey}
GET /api/v1/projects/{projectKey}/objects/{objectKey}
GET /api/v1/projects/{projectKey}/documents/{documentKey}
GET /api/v1/projects/{projectKey}/deliverables/{deliverableKey}
GET /api/v1/projects/{projectKey}/tasks/{taskKey}
GET /api/v1/projects/{projectKey}/tasks/{taskKey}/context
GET /api/v1/projects/{projectKey}/relations
GET /api/v1/projects/{projectKey}/impact?root={key}
```

### 5.2 Command APIs

Status/domain changes phải dùng commands thay vì generic patch:

```text
POST /tasks/{taskKey}/transitions
POST /tasks/{taskKey}/results
POST /artifacts
POST /verification-runs
POST /change-requests
POST /sync-proposals
```

Command model giúp backend validate permission, lifecycle, expected version và business rules.

### 5.3 Optimistic concurrency

External client phải gửi version/ETag/expectedVersion cho update nhạy cảm.

Ví dụ:

```json
{
  "transition": "start",
  "expectedVersion": 7
}
```

Nếu task đã thành version 8, server trả conflict thay vì overwrite.

### 5.4 Idempotency

External command có side effect cần hỗ trợ `Idempotency-Key`.

Ví dụ agent timeout sau khi submit result rồi retry; server phải trả cùng result thay vì tạo hai TaskResult.

## 6. Machine principal và authentication

### 6.1 Không dùng user credential cho agent

AI agent, CLI automation và CI integration phải dùng machine principal riêng.

```text
Principal(type=AIAgent/Service)
   ↓
ProjectMembership
   ↓
Role + Permission
   ↓
Credential
```

### 6.2 MVP API token

Token requirements:

- random high-entropy token;
- plaintext chỉ trả một lần;
- server lưu hash;
- optional expiration;
- revoke/rotate;
- project-scoped scopes;
- credential name/description;
- last-used metadata;
- full audit actor.

### 6.3 Scope examples

```text
project:read
object:read
document:read
deliverable:read
task:read
task:update-status
task:submit-result
artifact:create
verification:submit
change:create
```

Machine principal không mặc định có:

```text
member:manage
project:admin
baseline:approve
deliverable:accept
credential:manage
```

### 6.4 Future authentication

Sau MVP có thể hỗ trợ:

- OAuth2 Client Credentials;
- OIDC federation/workload identity;
- signed short-lived JWT;
- GitHub App identity.

Authorization vẫn dựa trên Principal + ProjectMembership + Permission/Scope.

## 7. AI task execution protocol

AI không cần đặc quyền domain riêng. External agent adapter có thể dùng protocol:

```text
1. Authenticate
2. GET task/{key}/context
3. Validate task Ready + baseline/version
4. POST transition=start
5. Execute work in repository/tooling
6. POST artifacts / result
7. POST transition=submit-for-review
8. Optional verification result ingestion
```

Task context bundle trả về:

```text
Task
Required Knowledge Object versions
Related Documents
Business Rules
Acceptance Criteria
Target Deliverables + versions
Allowed/expected scope
Dependencies
Verification requirements
Current Project Baseline
External repository bindings
```

Agent không nên tự crawl cả SaaS/repository để đoán requirement nếu task manifest đã định nghĩa context.

## 8. External artifact ingestion

### 8.1 Artifact mapping

Ví dụ:

```text
TASK-P2P-BE-042
   ↓ produces
Commit abc123
PR #381
src/Procurement/ApprovePurchaseOrderHandler.cs
openapi/procurement.yaml@abc123
```

SaaS không cần lưu full file content nếu source repository là canonical; chỉ lưu stable reference, revision và metadata/checksum cần thiết.

### 8.2 Webhook ingestion

Repository/CI webhook có thể cập nhật:

- PR opened/merged;
- commit pushed;
- workflow started/completed;
- test report available;
- deployment completed.

Webhook event phải map được về Project/Task/Deliverable bằng explicit metadata hoặc relation; không nên dựa hoàn toàn vào NLP từ commit message.

Có thể quy định commit/PR metadata:

```text
Task: TASK-P2P-BE-042
Deliverables: API-P2P-007, EVT-P2P-004
```

## 9. Change trigger sources

Change request có thể sinh từ:

```text
Human proposal
Requirement edit
Design review
External sync proposal
Production defect
Security finding
Dependency/platform upgrade
AI discovered issue
Verification failure
```

Source chỉ cho biết trigger; approval/governance workflow giữ nguyên.

## 10. Change lifecycle

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

Emergency path có thể bổ sung sau, nhưng vẫn phải tạo audit/change record hậu kiểm.

## 11. Impact engine

### 11.1 Graph-based impact discovery

Impact engine traverse graph từ changed object dựa trên relation policy.

Ví dụ:

```text
REQ-P2P-012 v4 → v5
   ↓ satisfied-by
DES-P2P-005
   ↓ introduces
API-P2P-007
   ↓ specifies
API-P2P-007-SPEC
   ↓ implements
TASK-P2P-BE-042
   ↓ verifies
VER-P2P-012
```

Không phải mọi edge đều propagate giống nhau. `RelationTypeDefinition` cần metadata:

```text
impactPropagationMode:
  NONE
  DIRECT
  TRANSITIVE
  REVIEW_ONLY
```

Có thể thêm direction và severity policy sau.

### 11.2 Impact result không phải automatic change

Graph chỉ trả `PotentialImpact`. Hệ thống không tự động kết luận mọi downstream object phải sửa.

Mỗi `ImpactItem` được disposition:

```text
UpdateRequired
ReviewRequired
RevalidationRequired
ReplanRequired
NoChangeRequired
Obsolete
```

### 11.3 Example

Requirement wording thay đổi nhưng API contract vẫn đáp ứng:

```text
REQ change
→ API spec: ReviewRequired
→ API deliverable: NoChangeRequired
→ Integration test: RevalidationRequired
```

Điều này tránh tạo task sửa code không cần thiết chỉ vì graph có edge.

## 12. Change application và document update

Một approved ChangeRequest phải trả lời được:

- document nào cần version mới;
- semantic object nào cần version mới;
- deliverable nào cần contract/version mới;
- task nào cần tạo/reopen/replan;
- verification nào phải chạy lại;
- milestone/release nào bị ảnh hưởng;
- export snapshot nào trở nên superseded.

Hệ thống cần hiển thị `Change Plan` trước khi apply.

Ví dụ:

```text
CR-2026-004
├── UPDATE REQ-P2P-012 v4 → v5
├── UPDATE API-P2P-007-SPEC v2 → v3
├── UPDATE Deliverable API-P2P-007 v2 → v3
├── CREATE TASK-P2P-BE-081
├── REVALIDATE VER-P2P-012
└── MARK Export EXP-103 stale
```

## 13. Stale dependency/version detection

Version-sensitive consumers phải record exact input version hoặc baseline.

```text
TASK-P2P-BE-042 input:
  REQ-P2P-012@v4
  API-P2P-007-SPEC@v2
```

Nếu current baseline chuyển sang v5/v3, task/result/verification cũ có thể được đánh dấu:

```text
CURRENT
STALE_REVIEW
STALE_REVALIDATE
SUPERSEDED
```

Stale không đồng nghĩa invalid; disposition quyết định hành động.

## 14. Import / two-way sync protocol

### 14.1 Không apply thẳng

External edits được parse thành `SyncProposal`:

```text
External files
   ↓ parse/validate identity
SyncProposal
   ↓ diff against source ExportSnapshot
Detected Changes
   ↓ conflict detection
ChangeRequest / Import Review
   ↓ approval
Apply canonical versions
```

### 14.2 Conflict classes

```text
NO_CONFLICT
APP_CHANGED_ONLY
EXTERNAL_CHANGED_ONLY
BOTH_CHANGED
UNKNOWN_BASELINE
IDENTITY_CONFLICT
SCHEMA_CONFLICT
```

`BOTH_CHANGED` không auto-merge ở governance layer nếu semantic merge không an toàn.

### 14.3 Identity rule

Import dựa trên technical ID/object key + export snapshot metadata, không dựa đơn thuần filename/title.

## 15. CLI direction

Một CLI tương lai có thể cung cấp:

```text
projgov login
projgov project pull
projgov project export
projgov task context TASK-P2P-BE-042
projgov task start TASK-P2P-BE-042
projgov task submit TASK-P2P-BE-042 --result result.json
projgov changes detect
projgov sync propose
```

CLI chỉ là API client; không chứa business rules độc lập với server.

## 16. Event/Webhook outbox

SaaS nên có domain/integration events để external systems subscribe:

```text
TaskReady
TaskAssigned
TaskStatusChanged
RequirementBaselined
DeliverableVersionChanged
ChangeRequestApproved
ImpactDetected
VerificationFailed
BaselineCreated
ExportSnapshotCreated
```

Outbound delivery cần retry và không được làm transaction core thất bại vì external endpoint down. Có thể dùng outbox pattern.

## 17. Security requirements

- Project boundary enforce ở mọi query/command.
- Token leak của agent A không được cho phép truy cập project B.
- Secret không xuất hiện trong export bundle/log.
- Webhook inbound cần signature validation hoặc equivalent provider authentication.
- External artifact URL phải được treat như untrusted metadata.
- Audit mọi machine command quan trọng.
- Rate limit và abuse protection cho public API.
- Credential rotation không làm mất principal/activity history.

## 18. MVP sequence

### Phase 1

- Export snapshot/manual download.
- Machine principal + scoped API token.
- Task context/read API.
- Task transition/result API.

### Phase 2

- GitHub repository binding.
- Push/PR export.
- Commit/PR/artifact mapping.
- CI run ingestion.

### Phase 3

- Change request + impact analysis.
- Stale version detection.
- Revalidation/replan actions.

### Phase 4

- External edit detection.
- Sync proposal.
- Reviewed two-way sync.

## 19. Nguyên tắc tổng kết

> **Integration không được phá vỡ ownership của dữ liệu. SaaS quản lý semantic project state; repository quản lý source code; CI quản lý execution logs; external actors tương tác thông qua identity, API, snapshot và change protocol rõ ràng.**

Nhờ boundary này, human, AI và tooling có thể làm việc tự do ở môi trường bên ngoài nhưng mọi thay đổi quan trọng vẫn được trace về project, version, task, deliverable và change decision tương ứng.
