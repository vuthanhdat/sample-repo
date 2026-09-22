# Generic Software Project Documentation Sample

`docs/sample-project/` là **sample structure tổng quát**, không đại diện cho bất kỳ nghiệp vụ cụ thể nào. Các tên `FEATURE-001`, `FEATURE-002`, `GOAL-001`... chỉ minh họa identity, relation và document model.

Các placeholder dạng `<...>` phải được thay bằng nội dung project thật.

## Structure

```text
sample-project/
├── 00-governance/
├── 10-goals/
├── 15-context/
├── 20-business-flows/
├── 25-features/
├── 30-requirements/
├── 40-design/
│   ├── 10-common/
│   └── 20-feature/
├── 50-deliverables/
│   ├── 00-lists/
│   ├── feature-001/
│   └── feature-002/
├── 55-planning/
├── 60-tasks/
├── 70-verification/
├── 80-operations/
├── 85-change-management/
└── 90-traceability/
```

## Core Model

```text
Goal
 ↓
Business Flow
 ↓
Feature
 ↓
Requirement + NFR/Data/Integration/Security Constraints
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

## Two Design Scopes

`40-design/10-common/` chứa technical baseline có thể áp dụng cho nhiều feature: architecture, backend/frontend, pagination/API rules, transaction/concurrency, data conventions, authentication/authorization, errors, logging/observability.

`40-design/20-feature/feature-NNN/` chứa design riêng cho feature: decision, API/screen/job/event/interface/data specs. Feature design kế thừa common baseline thay vì copy lại.

## Feature Index

`25-features/FEATURE-LIST.md` là index trung tâm. Folder `feature-001`, `feature-002` chỉ là projection theo stable feature ID; project thật đổi display name nhưng không cần đổi identity khi title thay đổi.

## Output Lists

`50-deliverables/00-lists/` có các list tổng hợp: Screen, API, Batch/Job, Interface, Event, Database, File, Mail, Notification, Report. Trong SaaS thật chúng nên được generate từ Deliverable Registry.

## Important Rules

1. Folder là navigation, graph relation mới là canonical model.
2. Không dùng domain giả để quyết định taxonomy.
3. Common Design và Feature Design là hai scope khác nhau.
4. Requirement nói WHAT; Design nói HOW; Deliverable là OUTPUT; Task là WORK; Verification là EVIDENCE definition/run.
5. Mọi traceable object có stable ID/version/status.
6. Type không applicable phải ghi `Not Applicable + reason`, không tạo fake output.
7. Task context được resolve từ relations thay vì để người/AI tự crawl và đoán.