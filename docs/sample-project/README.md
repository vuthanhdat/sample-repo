# Generic Software Project Documentation Sample

`docs/sample-project/` là **sample structure tổng quát** cho một software project, không đại diện cho bất kỳ nghiệp vụ cụ thể nào. Các tên `FEATURE-001`, `FEATURE-002`, `GOAL-001`... chỉ minh họa identity, relation và document model.

Các placeholder dạng `<...>` phải được thay bằng nội dung project thật. Folder là navigation; graph relation và stable ID mới là canonical model.

## Structure

```text
sample-project/
├── 00-governance/
├── 10-goals/
├── 15-context/
│   ├── PROJECT-CONTEXT.md
│   ├── SCOPE.md
│   ├── ACTOR-LIST.md
│   ├── EXTERNAL-SYSTEMS.md
│   ├── ASSUMPTIONS-CONSTRAINTS.md
│   └── GLOSSARY.md
├── 20-business-flows/
├── 25-features/
├── 30-requirements/
│   ├── functional/
│   ├── business-rules/
│   ├── data/
│   ├── integration/
│   ├── non-functional/
│   └── security/
├── 40-design/
│   ├── 10-common/
│   │   ├── architecture/
│   │   ├── backend/
│   │   ├── frontend/
│   │   ├── api/
│   │   ├── integration/
│   │   ├── data/
│   │   ├── security/
│   │   ├── infrastructure/
│   │   └── cross-cutting/
│   └── 20-feature/
│       └── feature-NNN/
│           ├── decision/
│           ├── domain/
│           ├── workflow-state/
│           ├── screen/
│           ├── api/
│           ├── job/
│           ├── event/
│           ├── integration/
│           ├── data/
│           ├── file/
│           ├── mail-notification/
│           └── report/
├── 50-deliverables/
│   ├── 00-lists/
│   ├── feature-001/
│   └── feature-002/
├── 55-planning/
├── 60-tasks/
├── 70-verification/
├── 80-operations/
│   ├── deployment/
│   ├── environments/
│   ├── observability/
│   ├── backup-recovery/
│   ├── runbooks/
│   └── release/
├── 85-change-management/
└── 90-traceability/
```

## Core Model

```text
Goal
 ↓
Context / Scope / Constraints
 ↓
Business Flow
 ↓
Feature
 ↓
Requirement + Business Rule + NFR/Data/Integration/Security Constraints
 ↓
Design
 ├── Common Design
 └── Feature Design
 ↓
Deliverable
 ↓
Task
 ↓
Verification
 ↓
Operations / Change / Traceability
```

## Documentation Principle

Document không chỉ là mô tả; nó phản ánh **các quyết định cần được đưa ra khi xây dựng và vận hành software**. Một vùng quyết định quá lớn nên được phân rã thành các decision area nhỏ hơn để có ownership, relation, review và change impact rõ ràng.

Ví dụ `architecture` không nên trở thành một mega-document chứa mọi thứ. Nó có thể được phân thành system structure, module structure, runtime interaction và deployment/infrastructure topology. Tương tự, backend, frontend, data và operations đều có taxonomy con.

Không bắt buộc project phải tạo mọi document. Mỗi loại phải được đánh giá `Applicable`, `Not Applicable + reason` hoặc chưa đánh giá; không tạo fake document chỉ để đủ cây thư mục.

## Two Design Scopes

`40-design/10-common/` chứa technical baseline áp dụng cho toàn application hoặc nhiều feature: architecture, backend/frontend, API, integration, data, security, infrastructure và cross-cutting concerns.

`40-design/20-feature/feature-NNN/` chứa design riêng cho feature. Ngoài output-facing design như screen/API/job/event/integration/data, feature design còn có `domain/` và `workflow-state/` để mô tả business model, invariants, lifecycle và state transition mà không nhầm chúng với database schema.

Feature design kế thừa common baseline thay vì copy lại.

## Feature Index

`25-features/FEATURE-LIST.md` là index trung tâm. Folder `feature-001`, `feature-002` chỉ là projection theo stable feature ID; project thật đổi display name nhưng không cần đổi identity khi title thay đổi.

## Output Lists

`50-deliverables/00-lists/` có các list tổng hợp: Screen, API, Batch/Job, Interface, Event, Database, File, Mail, Notification, Report. Trong hệ thống lớn chúng nên là derived views từ Deliverable Registry.

## Important Rules

1. Folder là navigation, graph relation mới là canonical model.
2. Không dùng domain giả để quyết định taxonomy.
3. Common Design và Feature Design là hai scope khác nhau.
4. Requirement nói WHAT; Design nói HOW; Deliverable là OUTPUT; Task là WORK; Verification là EVIDENCE definition/run.
5. Document taxonomy phải phản ánh các decision area độc lập; mega-document phải được breakdown khi chứa nhiều quyết định có lifecycle khác nhau.
6. Mọi traceable object có stable ID/version/status.
7. Type không applicable phải ghi `Not Applicable + reason`, không tạo fake output.
8. Task context được resolve từ relations thay vì để người/AI tự crawl và đoán.