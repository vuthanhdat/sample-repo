# Engineering Governance Platform

Repository này thiết kế một ứng dụng quản trị **project knowledge, task, deliverable/output, dependency, traceability và verification** cho các dự án phần mềm hoặc kỹ thuật có quy mô lớn.

Trọng tâm của sản phẩm không phải AI. **Human, AI Agent và Service Account đều là Member** của project; chúng có thể được giao task theo role/permission và tạo ra kết quả, nhưng project structure và governance không phụ thuộc vào việc người thực hiện là AI hay con người.

Mục tiêu là biến những thứ vốn thường nằm rời rạc trong Markdown, issue tracker, source code, CI và trí nhớ của team thành một mô hình có cấu trúc mà hệ thống có thể query, validate và trace hai chiều.

## 1. Core model

```text
Project
  ├── Members
  │     ├── Human
  │     ├── AI
  │     └── Service
  │
  ├── Knowledge
  │     ├── Documents
  │     └── Structured Knowledge Objects
  │            ├── Requirement
  │            ├── Business Rule
  │            ├── Acceptance Criterion
  │            ├── Design Decision
  │            └── Design Specification
  │
  ├── Deliverables / Outputs
  │     ├── Screen
  │     ├── API
  │     ├── Batch / Job
  │     ├── Event
  │     ├── Report
  │     ├── File
  │     └── Data / Configuration / Deployment Artifact
  │
  ├── Work
  │     ├── Roadmap
  │     ├── Milestone
  │     └── Task
  │
  ├── Traceability
  └── Verification / Evidence
```

Một task có thể được giao cho Human hoặc AI mà không thay đổi task schema:

```text
Task
   ↓ assigned-to
Member
```

AI chỉ khác ở execution adapter/capability, không phải ở domain model cốt lõi.

## 2. Ba separation quan trọng

### Knowledge không đồng nghĩa với Document

Document là container để đọc/viết. Requirement, Business Rule, Design Decision hoặc Specification là structured objects có ID, lifecycle, owner và relation riêng. Một document có thể chứa hoặc hiển thị nhiều object.

### Work không đồng nghĩa với Deliverable

Task mô tả công việc phải thực hiện. Deliverable mô tả output mà project cần tồn tại. Một task có thể implement nhiều deliverable có liên quan và một deliverable lớn có thể cần nhiều task.

### Verification Definition không đồng nghĩa với Evidence

Test definition chỉ mô tả cách kiểm chứng. Chỉ một verification run trên revision cụ thể cùng log/result/evidence mới chứng minh output đã được kiểm tra.

## 3. Traceability mục tiêu

Một graph điển hình:

```text
Business Flow
    ↓
Requirement
    ↓
Design Decision
    ↓
Deliverable
    ↓
Design Specification
    ↓
Task
    ↓
Implementation Artifact
    ↓
Verification Run
    ↓
Evidence
```

Relation là first-class data, không chỉ là hyperlink trong Markdown. Hệ thống phải trả lời được các câu hỏi như:

- Requirement này đã có design chưa?
- Design này tạo ra những deliverable nào?
- Deliverable nào chưa được implement hoặc verify?
- Task này tồn tại vì requirement/output nào?
- Task cần đọc những object nào trước khi làm?
- Human hay AI nào đang chịu trách nhiệm?
- Source artifact nào hiện thực API/screen/job này?
- Nếu một requirement hoặc contract thay đổi thì object/task/test nào bị ảnh hưởng?
- Milestone còn thiếu outcome nào?

## 4. Lifecycle tách biệt

Knowledge object:

```text
Draft → In Review → Baseline → Superseded
```

Deliverable:

```text
Planned → Specified → Implemented → Verified → Accepted → Deprecated
```

Task:

```text
Draft → Ready → In Progress → Review → Done
```

Task không mang trạng thái `Verified` hay `Accepted`; đó là trạng thái của output/deliverable.

## 5. Documentation trong repository

### [APP-REQUIREMENTS.md](./APP-REQUIREMENTS.md)

Định nghĩa mục tiêu sản phẩm, bounded functional areas, object model, screens, workflows, MVP, validation rules, permission và non-functional requirements.

### [APP-DOMAIN-MODEL.md](./APP-DOMAIN-MODEL.md)

Định nghĩa domain entities, lifecycle, relation model, baseline/versioning, logical data model và vị trí của AI trong architecture.

### [DESIGN-DELIVERABLE-TASK-GOVERNANCE.md](./DESIGN-DELIVERABLE-TASK-GOVERNANCE.md)

Tài liệu nền về quan hệ giữa design, deliverable, task, roadmap và traceability. Một số khái niệm trong tài liệu này cần tiếp tục được chuẩn hóa theo domain model mới, đặc biệt là lifecycle, source-of-truth của relation và separation giữa deliverable với implementation artifact.

## 6. Vai trò của AI

AI có ích trong project execution nhưng không được thiết kế thành authority trung tâm.

```text
Task Ready
    ↓
assigned-to Member(type=AI)
    ↓
Task Execution Adapter
    ↓
AI reads authorized task context
    ↓
TaskResult + Implementation Artifacts
    ↓
Review / Verification
    ↓
Done hoặc Rework
```

Cùng task đó có thể reassigned sang Human mà không thay đổi requirement, deliverable hay workflow cốt lõi.

AI cũng không phải cơ chế duy nhất quyết định output có đạt chuẩn hay không. Những rule có thể kiểm tra bằng compiler, analyzer, architecture test, unit/integration/E2E test, quality gate hoặc CI phải được enforce bằng tooling.

## 7. Technical direction ban đầu

Một kiến trúc MVP thực dụng:

```text
React + TypeScript
        ↓
ASP.NET Core API
        ↓
Modular Application / Domain
        ↓
PostgreSQL
```

PostgreSQL có thể lưu relation graph bằng relational edges và recursive CTE ở giai đoạn đầu. Chưa cần graph database cho MVP.

External tools đi qua adapter:

```text
Core Platform
    ↓ ports
Integration Adapters
    ├── GitHub / GitLab
    ├── CI/CD
    ├── Quality Tools
    └── AI Executor
```

Core project data phải tiếp tục hoạt động ngay cả khi các integration bên ngoài không khả dụng.

## 8. Product principle

> **Project knowledge, work và output là domain trung tâm. Human và AI chỉ là những member cùng tham gia thay đổi các domain object đó theo permission, lifecycle, traceability và verification do hệ thống quản lý.**
