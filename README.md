# Software Project Governance SaaS

Repository này thiết kế một **SaaS quản lý dự án phần mềm theo hướng traceability-first**. Sản phẩm thật không phải là bộ Markdown trong `docs/sample-project/`; sản phẩm là ứng dụng cho phép user tạo project, CRUD cấu trúc tài liệu, quản lý các đối tượng software-project có ID/version, quản lý quan hệ giữa chúng và phân tích ảnh hưởng khi một đối tượng thay đổi.

`docs/sample-project/` chỉ là **reference/acceptance fixture**: một ví dụ về bộ tài liệu mà ứng dụng phải có khả năng biểu diễn, tạo từ template, chỉnh sửa, export và trace. Không được dùng nội dung nghiệp vụ giả trong sample làm requirement của sản phẩm.

## 1. Product mental model

Core của sản phẩm gồm hai cấu trúc độc lập nhưng liên kết với nhau:

```text
A. Project Structure Tree

Project
└── Folder / Document nodes
    ├── Folder
    ├── Document
    └── Folder
        └── Document

→ CRUD structure, navigation, ordering, template instantiation, export path

B. Traceability Graph

Goal / Flow / Requirement / Rule / Design / Document
                  ↓ relations
Deliverable / Task / Verification / Artifact / Milestone / Change

→ dependency, coverage, backlinks, impact analysis, task context
```

**Vị trí của một file trong folder không phải là semantic relation.** Move/rename một document trong tree chỉ thay đổi navigation/export structure, trừ khi policy cụ thể quy định path là contract. Ngược lại, relation như `Requirement requires API`, `DesignSpecification specifies API`, `Task implements API` là semantic data và không được suy ra từ folder path.

## 2. SaaS model

```text
User
 ├── Project A
 ├── Project B
 └── Project C

Project
 ├── Members / RBAC
 ├── Structure Tree
 ├── Documents / Knowledge
 ├── Deliverables
 ├── Tasks / Roadmap
 ├── Relations / Traceability
 ├── Baselines / Versions
 ├── Changes / Impact
 ├── Verification
 └── Integrations
```

`User` là tài khoản SaaS toàn cục. Actor trong project được biểu diễn bằng `Principal` + `ProjectMembership`:

```text
Principal
├── Human
├── AI Agent
└── Service Account
```

AI chỉ là một project actor có machine identity, permission và credential riêng. Core project/task/document model không thay đổi tùy actor là human hay AI.

## 3. Project Template, Structure Template và Document Template

Khi tạo project, user có thể chọn một `ProjectTemplateVersion`. Template định nghĩa baseline policy và một **structure template tree** gồm folder/document template nodes.

Ví dụ:

```text
Software Project Standard
├── 00 Governance/
├── 10 Goals/
├── 20 Requirements/
├── 30 Design/
├── 40 Deliverables/
├── 50 Planning/
└── 60 Verification/
```

Khi instantiate project, structure template sinh ra `ProjectStructureNode` thực tế. Folder node chỉ quản lý cấu trúc; document node trỏ tới một `Document`.

`DocumentTemplateVersion` định nghĩa structure/content của một document cụ thể, ví dụ Requirement Specification, API Specification hoặc Test Strategy. Project template và document template là hai khái niệm khác nhau.

## 4. Document và semantic object

`Document` là authoring container có version riêng. Goal, Requirement, BusinessRule, DesignSpecification... là semantic objects có identity/version riêng. Một semantic object có thể được render trong nhiều document thông qua `KnowledgePlacement` mà không bị duplicate canonical state.

```text
Document Tree
   ↓
Document
   ↓ contains/renders
KnowledgePlacement
   ↓ references
KnowledgeObject
```

Document cũng là một traceable entity: có thể có relation với document khác hoặc với Requirement/Design/Deliverable khi relation đó có ý nghĩa semantic. Tuy nhiên `parent folder` hoặc `document nằm dưới folder X` vẫn thuộc Structure Tree, không dùng Relation để mô hình hóa.

## 5. Canonical traceability chain

Chuỗi điển hình:

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

Các reverse views như `specified-by`, `implemented-by`, `verified-by`, `required-by` được query/generated từ canonical edge, không lưu một edge editable thứ hai.

## 6. Identity và relation

Mọi traceable object có:

```text
Technical ID  → immutable UUID/ULID
Human Key     → stable, project-scoped key
```

Ví dụ:

```text
DOC-P2P-001
GOAL-001
REQ-P2P-012
DES-P2P-005
API-P2P-007
TASK-P2P-BE-042
VER-P2P-012
CR-2026-004
```

`Relation` là first-class canonical data và endpoint của relation dùng generic project-scoped object reference, không bị giới hạn ở `KnowledgeObject`.

Relation type định nghĩa ít nhất:

- allowed source types;
- allowed target types;
- canonical direction và reverse label;
- cycle policy;
- version sensitivity;
- impact propagation policy.

## 7. CRUD capability bắt buộc

MVP phải CRUD được ít nhất các nhóm sau:

- Project, membership, role/permission.
- Project Structure Node: create folder/document node, rename, move, reorder, archive/restore.
- Document: create from template, edit, version, baseline, history.
- Document section/placement và structured knowledge object.
- Deliverable/output và version/lifecycle.
- Task, dependency, roadmap/milestone.
- Relation: create, validate, update metadata, delete, query inbound/outbound/backlink.
- Verification definition/run/evidence.
- Change request, impact item, disposition.

Delete phải tôn trọng reference/version/audit policy; baseline object không được hard-delete hoặc silently overwrite.

## 8. Impact analysis

Khi một baseline/versioned entity thay đổi, hệ thống traverse Traceability Graph theo policy của từng relation type:

```text
Changed Entity
   ↓
Potential Impact Graph
   ↓
Document / Requirement / Design / Deliverable / Task / Verification / Milestone
   ↓
Impact disposition
├── UpdateRequired
├── ReviewRequired
├── RevalidationRequired
├── ReplanRequired
├── NoChangeRequired
└── Obsolete
```

Graph discovery chỉ tạo **potential impact**; hệ thống không tự kết luận mọi downstream entity phải sửa. User/owner phải disposition và ghi rationale. Exact version/baseline được dùng để phát hiện stale consumer.

## 9. External integration

SaaS là canonical source cho governance/semantic project state. Source repository vẫn là source of truth cho code, CI provider là source of truth cho CI execution result.

```text
SaaS
  ↓ export snapshot
.project-governance/ in repository
  ↓ human / AI / tooling
Source code / PR / CI
  ↓ API / webhook
SaaS artifact / task result / verification / change data
```

Two-way sync không được silently overwrite canonical state; external edits phải đi qua identity/version comparison, conflict detection và change/sync proposal.

## 10. Lifecycle

Knowledge object:

```text
Draft → InReview → Baseline → Superseded
```

Deliverable:

```text
Planned → Specified → Implemented → Verified → Accepted → Deprecated
```

Task:

```text
Draft → Ready → InProgress → Review → Done
          ↕          ↕
        Blocked    Blocked
```

Task và Deliverable có lifecycle độc lập. `Task Done` không đồng nghĩa `Deliverable Accepted`.

## 11. Canonical documents

- [APP-REQUIREMENTS.md](./APP-REQUIREMENTS.md): product requirements và acceptance boundary.
- [APP-DOMAIN-MODEL.md](./APP-DOMAIN-MODEL.md): canonical domain concepts và invariants.
- [docs/DOCUMENT-ENTITY-MODEL.md](./docs/DOCUMENT-ENTITY-MODEL.md): document/structure/placement model và boundary với traceability graph.
- [DESIGN-DELIVERABLE-TASK-GOVERNANCE.md](./DESIGN-DELIVERABLE-TASK-GOVERNANCE.md): vocabulary và governance từ requirement → design → deliverable → task → verification.
- [INTEGRATION-SYNC-CHANGE-MANAGEMENT.md](./INTEGRATION-SYNC-CHANGE-MANAGEMENT.md): integration, export/sync, baseline, stale detection và impact protocol.
- [`docs/sample-project/`](./docs/sample-project/): **non-canonical sample fixture**, dùng để kiểm tra khả năng biểu diễn/export của app.

Thứ tự ưu tiên khi có mâu thuẫn: `APP-REQUIREMENTS.md` → `APP-DOMAIN-MODEL.md` → tài liệu chuyên đề. Sample project không bao giờ override product requirement/domain model.

## 12. Acceptance fixture: sample-project

Ứng dụng được coi là đủ nền tảng cho design/code khi có thể dùng UI/API để:

1. tạo một project template có structure tương đương `docs/sample-project/`;
2. instantiate project và sinh folder/document tree tương ứng;
3. CRUD/move/reorder document mà không làm mất stable identity;
4. tạo Requirement, Design, Deliverable, Task, Verification và đặt chúng vào document phù hợp;
5. tạo/query relation hai chiều giữa các traceable entities;
6. đổi một baseline Requirement và xem được potential impact tới document/design/deliverable/task/verification;
7. export lại project thành một structure machine-readable + human-readable có stable IDs/version.

## 13. Technical direction

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

PostgreSQL lưu canonical entity state và relation edges. Recursive CTE/index đủ cho MVP; chỉ cân nhắc graph database khi có evidence về nhu cầu scale/query mà relational model không đáp ứng được.

> **Core product là một project-governance graph có authoring tree, stable identity, version và relations. Document chỉ là một trong các traceable entities; folder tree là navigation structure; traceability graph mới là nền tảng để coverage, task context và impact analysis hoạt động.**