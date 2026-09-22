# Design, Deliverable, Task và Traceability Governance

## 1. Mục đích

Trong một dự án lớn, đặc biệt khi sử dụng AI để phân tích, thiết kế và viết code, vấn đề không chỉ là tài liệu có đầy đủ hay không mà là toàn bộ chuỗi từ yêu cầu đến sản phẩm cuối cùng có được kiểm soát hay không. Một hệ thống có thể có rất nhiều file Markdown, rất nhiều sơ đồ và một `AGENTS.md` chi tiết nhưng vẫn thất bại nếu không trả lời được một cách máy móc các câu hỏi: requirement nào sinh ra màn hình này, design nào quy định API này, task nào phải implement batch này, task đó phải đọc những tài liệu nào, task phụ thuộc vào task nào, bằng chứng nào xác nhận task đã hoàn thành, và nếu một requirement thay đổi thì những design, deliverable, task và test nào bị ảnh hưởng.

Tài liệu này định nghĩa một mô hình thống nhất cho **document structure, design product, system deliverable, task, tasklist, dependency, roadmap và traceability**. Mục tiêu là biến documentation từ một kho Markdown thành một hệ thống knowledge có cấu trúc, đồng thời biến tasklist từ danh sách việc làm thủ công thành execution model có thể giao trực tiếp cho con người hoặc AI agent.

Nguyên tắc trung tâm là:

> **Requirement định nghĩa điều hệ thống phải đảm bảo; design định nghĩa hình dạng và contract của giải pháp; deliverable là sản phẩm hệ thống phải tồn tại; task là đơn vị công việc để tạo hoặc thay đổi deliverable; verification cung cấp bằng chứng rằng requirement và design đã được hiện thực đúng.**

Toàn bộ chuỗi phải trace được hai chiều.

```text
Business Goal
    ↓
Business Capability / Business Flow
    ↓
Requirement
    ↓
Design Product / Specification
    ↓
Required Deliverable
    ↓
Task / Task Dependency
    ↓
Implementation Output
    ↓
Verification Evidence
    ↓
Accepted Baseline
```

## 2. Phân biệt các khái niệm cốt lõi

### 2.1 Requirement

Requirement mô tả **WHAT must be true**. Requirement không nên chứa chi tiết implementation trừ khi chi tiết đó thực sự là constraint bắt buộc của business hoặc platform. Một requirement có thể là functional requirement, business rule, data requirement, integration requirement hoặc non-functional requirement.

Ví dụ `REQ-P2P-012` có thể quy định rằng Purchase Order chỉ được phát hành sau khi được phê duyệt. Requirement này không nên tự quyết định controller nào, table nào hoặc library nào sẽ được dùng.

### 2.2 Design Document

Design document là **container của knowledge và quyết định thiết kế**. Một document có thể chứa nhiều design product, nhưng document không nên được coi là deliverable runtime của hệ thống. Ví dụ `purchase-order-design.md` là document; bên trong nó có thể mô tả screen specification, API specification, state transition và event contract.

Document trả lời các câu hỏi như: tại sao chọn thiết kế này, boundary ở đâu, responsibility thuộc component nào, contract là gì, constraint nào cần giữ, và các design product liên quan với nhau ra sao.

### 2.3 Design Product

Design product là một **đối tượng thiết kế có ID, owner, lifecycle và contract riêng**, có thể nằm trong Markdown, YAML hoặc một công cụ modeling. Đây là cấp độ cần trace, không phải chỉ trace file Markdown.

Ví dụ:

- `SCR-P2P-002-SPEC`: Screen Specification cho Purchase Order Detail.
- `API-P2P-007-SPEC`: API Contract cho Approve Purchase Order.
- `JOB-P2P-001-SPEC`: Batch/Job Specification cho Auto Close Purchase Order.
- `EVT-P2P-004-SPEC`: Event Contract cho PurchaseOrderApproved.
- `DATA-P2P-001-SPEC`: Logical Data Specification cho PurchaseOrder.

Design product trả lời câu hỏi **deliverable phải trông như thế nào và phải tuân contract gì**.

### 2.4 System Deliverable

System deliverable là sản phẩm mà hệ thống cuối cùng phải có hoặc phải phát sinh trong runtime/deployment. Đây là “hình dạng” thực tế của hệ thống và phải được kiểm kê từ requirement/design thay vì để implementation tự phát minh.

Các loại deliverable điển hình gồm:

| Loại | Ví dụ |
|---|---|
| Screen | Purchase Order Detail, Employee List |
| API | Approve Purchase Order API |
| Batch / Job | Month-end Closing Job |
| File | Purchase Order PDF, Bank Transfer CSV |
| Report | Trial Balance, P&L |
| Event | PurchaseOrderApproved |
| Notification | Approval Request Notification |
| Interface | Supplier Integration, Bank Integration |
| Data Object | PurchaseOrder, JournalEntry |
| Database Artifact | Table, View, Migration |
| Configuration | Approval policy, code master, feature configuration |

Một deliverable phải có ID ổn định, owner, requirement nguồn, design product quy định nó, task hiện thực nó và verification chứng minh nó hoạt động đúng.

### 2.5 Task

Task là **đơn vị thực thi**, không phải requirement và cũng không phải design. Task tồn tại để tạo, sửa, di chuyển hoặc loại bỏ một hay nhiều deliverable theo một design đã được xác định.

Ví dụ `TASK-P2P-BE-042` có thể implement `API-P2P-007` và publish `EVT-P2P-004`. Task không được tự quyết định rằng hai deliverable đó nên tồn tại; quyết định này phải có từ requirement/design trước khi task ở trạng thái Ready.

### 2.6 Implementation Output và Verification Evidence

Implementation output là thay đổi thực tế trong codebase, database migration, workflow definition, configuration hoặc deployment artifact. Verification evidence là bằng chứng có thể kiểm tra được như unit test, integration test, contract test, E2E test, architecture test, static-analysis result hoặc quality-gate result.

Do đó “task hoàn thành” không đồng nghĩa với “AI đã viết code”. Task chỉ hoàn thành khi output đã tồn tại, traceability đầy đủ và các verification bắt buộc đã pass.

## 3. Quan hệ chuẩn giữa Requirement, Design, Deliverable và Task

Mô hình quan hệ nên được định nghĩa rõ bằng các relation có nghĩa thay vì chỉ dùng hyperlink tùy ý.

```text
Requirement --requires----------> Deliverable
Design      --specifies---------> Deliverable
Task        --implements--------> Deliverable
Task        --modifies----------> Deliverable
Task        --depends-on--------> Task / Deliverable
Test        --verifies----------> Requirement / Deliverable
Deliverable --owned-by----------> System / Module
Deliverable --participates-in---> Business Flow
Milestone   --contains----------> Task
Roadmap     --contains----------> Milestone / Phase
```

Ví dụ:

```text
BF-P2P-001
   ↓
REQ-P2P-012
   ├── requires → SCR-P2P-003
   ├── requires → API-P2P-007
   └── requires → EVT-P2P-004

SCR-P2P-003-SPEC ── specifies → SCR-P2P-003
API-P2P-007-SPEC ── specifies → API-P2P-007
EVT-P2P-004-SPEC ── specifies → EVT-P2P-004

TASK-P2P-FE-041 ── implements → SCR-P2P-003
TASK-P2P-BE-042 ── implements → API-P2P-007, EVT-P2P-004

E2E-P2P-012 ── verifies → REQ-P2P-012, SCR-P2P-003
IT-P2P-012  ── verifies → API-P2P-007, EVT-P2P-004
```

Mô hình này cho phép kiểm tra cả hai chiều. Từ requirement phải đi xuống được deliverable, task và test; từ một file code hoặc một API cụ thể phải lần ngược lên được task, design, requirement và business flow đã tạo ra nó.

## 4. Cấu trúc document và source of truth

Repository không nên tổ chức chỉ theo loại file Markdown mà nên tổ chức theo **knowledge layer và artifact registry**. Một cấu trúc tham khảo:

```text
/docs
  /00-governance
    document-model.md
    traceability-rules.md
    task-governance.md
    quality-gates.md

  /10-business
    /goals
    /capabilities
    /actors
    /business-rules
    /flows

  /20-requirements
    /functional
    /data
    /integration
    /non-functional

  /30-design
    /systems
    /screens
    /apis
    /jobs
    /files
    /reports
    /events
    /interfaces
    /data
    /security
    /architecture

  /40-execution
    roadmap.yaml
    milestones.yaml
    tasklist.yaml
    /tasks

  /50-traceability
    artifact-registry.yaml
    relations.yaml
    coverage-report.md

  /60-verification
    acceptance-matrix.yaml
    test-strategy.md
```

Không bắt buộc mọi project phải dùng đúng tên folder trên, nhưng phải giữ được separation of concern. Business knowledge không nên lẫn với implementation task; requirement không nên lẫn với detailed design; design không nên bị copy vào task; task không nên trở thành nơi định nghĩa lại business rule.

Mỗi khái niệm chỉ có một canonical source. Nếu `BR-P2P-006` đã được định nghĩa trong business-rule registry thì screen spec, API spec và task chỉ reference `BR-P2P-006`, không copy nguyên nội dung sang ba nơi khác nhau. Điều này giảm divergence và cho phép impact analysis khi rule thay đổi.

## 5. Design Product Inventory: kiểm soát sản phẩm thiết kế

Một design phase không được coi là hoàn thành chỉ vì đã có “một tài liệu design”. Phải kiểm kê được **design product nào bắt buộc phải có** dựa trên loại deliverable.

Ví dụ một chức năng có screen, API, event và data object thì design inventory tối thiểu phải có:

| Design Product | Deliverable được quy định |
|---|---|
| Screen Specification | Screen |
| UI Item / Action Specification | Screen items và actions |
| API Contract | API |
| Business Rule Mapping | API và UI behavior |
| Event Contract | Event |
| Logical Data Specification | Data object |
| Authorization Specification | Screen/API permissions |
| Acceptance Criteria | Requirement behavior |

Một batch/job cần product khác: schedule/trigger, selection condition, transaction boundary, retry, idempotency, concurrency, partial-failure behavior, logging, metrics và rerun policy. Một file cần schema/layout, naming, encoding, producer/consumer, generation timing, retention và versioning. Một report cần dimensions, measures, calculation rules, cutoff rule, rounding, drilldown và export format.

Nhờ design product inventory, project có thể phát hiện “design gap” trước khi code. Nếu requirement đã yêu cầu một file output nhưng chưa có `FILE-xxx-SPEC`, task implementation cho file đó chưa được phép chuyển sang Ready.

## 6. Deliverable Inventory: kiểm soát hình dạng hệ thống

Deliverable inventory là danh sách canonical tất cả output bắt buộc của hệ thống. Nó không chỉ phục vụ documentation mà là input cho planning và traceability.

Ví dụ:

```yaml
- id: SCR-P2P-003
  type: screen
  name: Purchase Order Approval
  owner: SYS-PROCUREMENT
  requiredBy:
    - REQ-P2P-012
  specifiedBy:
    - SCR-P2P-003-SPEC
  implementedBy:
    - TASK-P2P-FE-041
  verifiedBy:
    - E2E-P2P-012

- id: API-P2P-007
  type: api
  name: Approve Purchase Order
  owner: SYS-PROCUREMENT
  requiredBy:
    - REQ-P2P-012
  specifiedBy:
    - API-P2P-007-SPEC
  implementedBy:
    - TASK-P2P-BE-042
  verifiedBy:
    - IT-P2P-012
```

Các rule có thể tự động kiểm tra gồm: deliverable phải có requirement cha; deliverable phải có owner; deliverable phải có design spec phù hợp; deliverable ở trạng thái Implemented phải có ít nhất một task; deliverable ở trạng thái Accepted phải có verification; không được có screen/API/job/file/event “orphan” do implementation tự tạo mà không có nguồn yêu cầu.

## 7. Task là execution contract

Task phải được thiết kế như một contract giữa project và người/AI thực thi. Một task tốt phải trả lời chính xác năm câu hỏi: **tại sao làm, phải đọc gì, được phép thay đổi gì, phải tạo ra gì, và chứng minh hoàn thành bằng cách nào**.

Một task manifest tham khảo:

```yaml
id: TASK-P2P-BE-042
title: Implement Purchase Order Approval API
type: backend
milestone: MS-P2P-02
priority: high

objective:
  requirement: REQ-P2P-012
  description: Implement server-side approval behavior for Purchase Order.

traceability:
  businessFlows:
    - BF-P2P-001
  requirements:
    - REQ-P2P-012
  businessRules:
    - BR-P2P-006
    - BR-P2P-007

readSet:
  mandatory:
    - REQ-P2P-012
    - BR-P2P-006
    - BR-P2P-007
    - API-P2P-007-SPEC
    - EVT-P2P-004-SPEC
    - DATA-P2P-001-SPEC
    - AC-P2P-012
  standards:
    - ARCH-CLEAN-001
    - API-STANDARD-001
    - SEC-RBAC-001

writeSet:
  deliverables:
    - id: API-P2P-007
      action: implement
    - id: EVT-P2P-004
      action: implement
  allowedPaths:
    - src/Procurement/Application/**
    - src/Procurement/Domain/**
    - src/Procurement/Api/**
    - tests/Procurement/**
  forbiddenPaths:
    - src/Accounting/**
    - src/IAM/**

verifySet:
  acceptanceCriteria:
    - AC-P2P-012-01
    - AC-P2P-012-02
    - AC-P2P-012-03
  tests:
    - UT-P2P-012
    - IT-P2P-012
  qualityGates:
    - build
    - unit-test
    - integration-test
    - architecture-test
    - sonar-quality-gate

dependsOn:
  tasks:
    - TASK-P2P-DATA-030
  deliverables:
    - DATA-P2P-001

doneWhen:
  - declared deliverables are implemented
  - acceptance criteria pass
  - required quality gates pass
  - no out-of-scope artifact is changed
  - traceability validation passes
```

`readSet` là context bắt buộc AI phải resolve trước khi code; `writeSet` giới hạn phạm vi thay đổi; `verifySet` định nghĩa bằng chứng cần tạo và gate phải vượt qua. Cấu trúc này quan trọng hơn việc viết một prompt rất dài, vì prompt chỉ nên truyền Task ID và command thực thi, còn knowledge và constraint được resolve từ manifest.

## 8. Task Type và Read Policy

Không phải mọi task đều cần đọc tất cả document. Đọc quá nhiều context làm tăng noise và có thể khiến AI trộn responsibility. Vì vậy project nên định nghĩa policy theo task type.

| Task type | Mandatory input điển hình |
|---|---|
| Frontend | Requirement, screen spec, UI item/action spec, API contract, permission spec, acceptance criteria, UI standard |
| Backend | Requirement, business rules, API contract, data spec, event spec, authorization rule, architecture rule, acceptance criteria |
| Batch/Job | Requirement, job spec, business rules, data spec, schedule, retry/error policy, observability policy, acceptance criteria |
| Integration | Requirement, interface contract, event/file contract, idempotency/error policy, acceptance criteria |
| Database | Data requirement, logical data spec, ownership, retention/audit rule, DB/migration standard |
| Test | Requirement, acceptance criteria, relevant contracts, test strategy, environment/data setup |
| Refactoring | Architecture rule, current dependency map, affected contracts, regression tests, explicit non-functional goal |

Task validator có thể từ chối chuyển task sang Ready nếu input bắt buộc theo loại task còn thiếu. Điều này đặc biệt hữu ích khi làm việc với AI vì nó ngăn agent bắt đầu implementation khi specification chưa hoàn thiện.

## 9. Tasklist không phải danh sách TODO

Tasklist là **execution model được derive từ deliverable inventory và dependency graph**, không phải danh sách việc được nghĩ ra theo cảm hứng. Mỗi task phải có ID, type, owner/agent role, milestone, dependency, read/write/verify set, status và traceability.

Tasklist cấp project nên chứa metadata đủ để planning mà không copy toàn bộ task manifest:

```yaml
- id: TASK-P2P-DATA-030
  type: database
  milestone: MS-P2P-01
  status: ready
  implements:
    - DATA-P2P-001
  dependsOn: []

- id: TASK-P2P-BE-042
  type: backend
  milestone: MS-P2P-02
  status: blocked
  implements:
    - API-P2P-007
    - EVT-P2P-004
  dependsOn:
    - TASK-P2P-DATA-030

- id: TASK-P2P-FE-041
  type: frontend
  milestone: MS-P2P-02
  status: blocked
  implements:
    - SCR-P2P-003
  dependsOn:
    - TASK-P2P-BE-042

- id: TASK-P2P-E2E-050
  type: test
  milestone: MS-P2P-03
  status: blocked
  verifies:
    - REQ-P2P-012
  dependsOn:
    - TASK-P2P-FE-041
    - TASK-P2P-BE-042
```

Nhờ cấu trúc này, tasklist có thể được validate và visualized thành dependency graph thay vì chỉ là checklist.

## 10. Dependency phải được mô hình hóa ở nhiều cấp

Chỉ có dependency giữa task với task là chưa đủ. Dự án enterprise có ít nhất bốn loại dependency cần phân biệt.

**Business dependency** thể hiện flow/capability nào cần flow/capability khác tồn tại trước. Ví dụ Invoice Matching phụ thuộc Purchase Order và Goods Receipt. **Design dependency** thể hiện một design product cần contract khác ổn định trước khi hoàn thiện, ví dụ screen design phụ thuộc API contract và data dictionary. **Deliverable dependency** thể hiện runtime artifact phụ thuộc artifact khác, ví dụ frontend screen phụ thuộc API, posting job phụ thuộc accounting period configuration. **Execution dependency** là dependency giữa task, dùng để lập lịch thực thi.

Không nên suy luận execution dependency chỉ từ thứ tự task được ghi trong file. Dependency phải explicit:

```text
TASK-P2P-DATA-030
        ↓
TASK-P2P-BE-042
        ↓
TASK-P2P-FE-041
        ↓
TASK-P2P-E2E-050
```

Đồng thời không nên tạo dependency giả. Frontend và backend có thể phát triển song song nếu API contract đã baseline và frontend sử dụng mock/contract fixture. Khi đó dependency thật là cả hai task cùng phụ thuộc `API-P2P-007-SPEC`, chứ frontend không nhất thiết phải chờ backend code xong.

```text
                 API-P2P-007-SPEC
                  /             \
                 ↓               ↓
        TASK-P2P-FE-041   TASK-P2P-BE-042
                  \             /
                   ↓           ↓
                    E2E / Integration
```

Cách mô hình dependency đúng sẽ trực tiếp quyết định khả năng parallelize nhiều AI agent mà không gây conflict hoặc build sai contract.

## 11. Roadmap, Phase và Milestone

Roadmap trả lời câu hỏi **khi nào và theo thứ tự chiến lược nào các capability/deliverable được đưa vào baseline**. Tasklist trả lời câu hỏi **công việc cụ thể nào phải được thực hiện**. Hai thứ liên quan chặt nhưng không được trộn làm một.

Một hierarchy thực dụng:

```text
Roadmap
  └── Phase / Release
       └── Milestone
            └── Task
                 └── Deliverable changes
```

Ví dụ:

```text
ROADMAP ERP-LAB

Phase 1 - Procurement Foundation
  MS-P2P-01 Data and Core Domain
  MS-P2P-02 PO Transaction Flow
  MS-P2P-03 Verification and E2E

Phase 2 - Accounting Integration
  MS-ACC-01 Posting Contract
  MS-ACC-02 Journal Integration
  MS-ACC-03 Reconciliation
```

Milestone phải được định nghĩa bằng **outcome/deliverable set**, không chỉ bằng ngày hoặc danh sách task. Ví dụ `MS-P2P-02` hoàn thành khi `SCR-P2P-003`, `API-P2P-007` và `EVT-P2P-004` đạt trạng thái Verified. Task là phương tiện để đạt milestone; nếu task decomposition thay đổi nhưng deliverable outcome không đổi thì roadmap không nên bị viết lại toàn bộ.

Roadmap cũng phải tôn trọng dependency graph. Nếu Phase 2 cần accounting event contract từ Phase 1 thì dependency đó phải explicit. Công cụ planning có thể dùng graph để tìm critical path, các task có thể chạy song song và các blocker thực sự.

## 12. Definition of Ready

Một task chỉ được giao cho AI khi ở trạng thái Ready. Definition of Ready nên được kiểm tra tự động càng nhiều càng tốt.

Một task implementation tối thiểu cần thỏa mãn: requirement tồn tại và đã baseline; deliverable cần implement đã được declare; design product bắt buộc đã có và không ở trạng thái Draft chưa kiểm soát; business rule và contract dependency đã resolve; readSet đầy đủ; writeSet và allowed scope rõ ràng; acceptance criteria tồn tại; dependency task/deliverable đã đạt trạng thái yêu cầu; quality gate đã được xác định.

Nếu thiếu API contract, thiếu data ownership hoặc acceptance criteria còn mơ hồ, task phải ở `Blocked` hoặc `Draft`, không nên để AI tự suy luận phần còn thiếu và tiếp tục code.

Trạng thái tham khảo:

```text
Draft → Ready → In Progress → Implemented → Verified → Accepted
           ↑          |
           └─ Blocked ┘
```

`Implemented` chỉ nói output đã được tạo; `Verified` nói verification bắt buộc đã pass; `Accepted` nói output đã được đưa vào baseline/release theo governance của project.

## 13. Definition of Done

Definition of Done phải gắn với traceability và verification, không chỉ với việc build thành công. Một task chỉ được Done khi toàn bộ declared deliverable đã được implement hoặc modified đúng action; required tests đã tồn tại; acceptance criteria pass; architecture/static/security/quality gates bắt buộc pass; không có file hoặc artifact ngoài writeSet bị thay đổi nếu không có approved change request; documentation/registry được cập nhật nếu contract thay đổi; và traceability validator không phát hiện missing link.

Với AI coding, một câu trả lời như “implementation completed successfully” không phải evidence. Evidence phải nằm trong repository hoặc CI result.

## 14. AI Execution Protocol

Khi giao việc cho AI, prompt không nên mang toàn bộ requirement/design. Một command lý tưởng chỉ cần xác định Task ID và yêu cầu tuân execution protocol.

```text
Implement TASK-P2P-BE-042.
```

Agent runner phải resolve task manifest, đọc toàn bộ `readSet.mandatory`, kiểm tra dependency và Definition of Ready, chỉ thay đổi artifact/path trong writeSet, tạo output/test được khai báo trong deliverables và verifySet, chạy quality gates, sau đó cập nhật task status cùng traceability evidence.

`AGENTS.md` vì thế nên đóng vai trò hướng dẫn protocol chung, ví dụ: “khi nhận Task ID, resolve manifest; không code nếu task chưa Ready; không tự tạo deliverable ngoài writeSet; mọi assumption làm thay đổi contract phải được ghi thành change request; chỉ kết luận Done khi verifySet và quality gate pass.” Knowledge nghiệp vụ cụ thể không nên copy vào `AGENTS.md`.

## 15. Change Request và kiểm soát việc AI tự mở rộng scope

Trong quá trình implementation, agent có thể phát hiện design chưa đủ hoặc cần thêm deliverable. Việc này không nên bị cấm tuyệt đối, nhưng agent không được âm thầm tạo artifact mới. Nó phải tạo hoặc đề xuất một **Design/Requirement Change Request**.

Ví dụ task chỉ cho phép implement `API-P2P-007`, nhưng trong quá trình làm AI nhận ra cần thêm `JOB-P2P-009`. Trạng thái đúng là task bị block hoặc tiếp tục phần không phụ thuộc, đồng thời tạo change request mô tả lý do, requirement bị ảnh hưởng, design product mới cần bổ sung, dependency mới và impact lên roadmap/tasklist. Chỉ sau khi change được baseline thì deliverable/task mới được thêm vào registry.

Cơ chế này phân biệt rõ “AI phát hiện vấn đề hợp lý” với “AI tự vẽ thêm hệ thống”.

## 16. Version, Baseline và Impact Analysis

Traceability chỉ có giá trị nếu biết task đã đọc phiên bản nào của requirement/design. Task nên record input baseline hoặc revision hash cho các input quan trọng.

```yaml
inputBaseline:
  REQ-P2P-012: rev-04
  API-P2P-007-SPEC: rev-02
  DATA-P2P-001-SPEC: rev-07
```

Khi `REQ-P2P-012` đổi sang `rev-05`, hệ thống có thể query graph để tìm toàn bộ design product, deliverable, task và test phụ thuộc revision cũ. Các task đã Accepted không nhất thiết tự động quay lại In Progress, nhưng phải tạo impact-analysis item hoặc revalidation requirement.

Đây là khác biệt giữa “có link trong Markdown” và “có traceability thực sự”. Traceability phải hỗ trợ change propagation.

## 17. Traceability Matrix và các kiểm tra bắt buộc

Ngoài graph, nên sinh ra matrix để con người review nhanh:

| Requirement | Design | Deliverable | Task | Verification | Status |
|---|---|---|---|---|---|
| REQ-P2P-012 | SCR/API/EVT specs | SCR-003, API-007, EVT-004 | FE-041, BE-042 | IT-012, E2E-012 | Verified |
| REQ-P2P-020 | JOB spec | JOB-001 | JOB-060 | IT-020 | In Progress |

Các validator quan trọng gồm: requirement không có deliverable; deliverable không có requirement; deliverable không có design spec; deliverable không có implementation task; task không có requirement hoặc objective; task thiếu mandatory input; task phụ thuộc vào cycle; task Ready nhưng dependency chưa Ready/Accepted theo policy; task Done nhưng verifySet chưa pass; test không trace về requirement/deliverable; implementation artifact không có task nguồn; design product không còn được deliverable nào dùng; requirement thay đổi sau baseline nhưng dependent deliverable chưa được revalidated.

Những check này nên được đưa vào CI hoặc một traceability checker thay vì review thủ công toàn bộ.

## 18. Roadmap và Tasklist được sinh ra từ design như thế nào

Quy trình planning nên đi theo thứ tự sau. Đầu tiên xác định business scope và business flow. Tiếp theo phân rã thành requirement và business rule. Từ requirement xác định required deliverable inventory. Với từng deliverable, tạo design product cần thiết và dependency giữa các contract. Khi design đạt mức đủ để implementation, phân rã mỗi deliverable thành task theo boundary kỹ thuật hợp lý. Sau đó xây execution dependency graph, nhóm task vào milestone theo outcome, rồi nhóm milestone vào phase/release của roadmap.

Chuỗi này có thể biểu diễn như sau:

```text
Business Scope
    ↓
Business Flow
    ↓
Requirement Set
    ↓
Deliverable Inventory
    ↓
Design Product Inventory
    ↓
Design Dependency Graph
    ↓
Task Decomposition
    ↓
Execution Dependency Graph
    ↓
Milestones
    ↓
Roadmap / Release Plan
```

Điểm quan trọng là tasklist **không nên xuất hiện trước khi biết deliverable cần tạo**. Nếu bắt đầu bằng “hãy tạo 50 task cho module HCM”, AI rất dễ invent công việc và kiến trúc. Ngược lại, nếu đã có deliverable inventory và design dependency, tasklist chỉ là decomposition của execution work nên dễ kiểm soát hơn rất nhiều.

## 19. Quan hệ many-to-many giữa Task và Deliverable

Không nên ép quan hệ một task bằng một deliverable. Một backend task có thể implement một API và một domain event nếu chúng là một unit of change hợp lý; một deliverable lớn như report phức tạp có thể cần nhiều task gồm data query, backend contract, frontend rendering và performance tuning.

Do đó relation phải hỗ trợ many-to-many nhưng cần giữ task đủ nhỏ để có thể verify độc lập. Quy tắc thực dụng là một task nên có một objective thống nhất và một writeSet đủ nhỏ để review. Nếu task chạm nhiều module, nhiều loại deliverable không liên quan hoặc cần nhiều acceptance boundary khác nhau, task nên được tách.

## 20. Ownership và Boundary

Mọi requirement, design product và deliverable cần có owner ở mức system/module/domain. Ownership giúp quyết định nơi đặt canonical source và ngăn việc một task trong module này tự sửa data hoặc contract thuộc module khác.

Ví dụ `DATA-CUSTOMER-001` thuộc CRM. Order Management có thể reference Customer ID hoặc snapshot theo contract nhưng không được tự thay schema master customer. Nếu task Order cần thay đổi Customer contract, dependency phải đi qua change request hoặc contract task của CRM.

Ownership cũng nên được dùng trong code ownership, architecture tests và CI policy để biến document rule thành enforcement.

## 21. Machine-readable trước, Markdown để giải thích

Những dữ liệu mang tính registry và relation như ID, type, owner, status, dependency, requiredBy, specifiedBy, implementedBy, verifiedBy, milestone và baseline revision nên được lưu ở dạng machine-readable như YAML/JSON hoặc database/modeling tool. Markdown phù hợp cho rationale, context, diagrams, trade-off và explanation.

Một nguyên tắc hữu ích:

> **Nếu thông tin cần được validate, query, graph, diff hoặc dùng để quyết định AI phải đọc gì, thông tin đó không nên chỉ tồn tại trong prose.**

Ví dụ tên task và dependency nên ở YAML; lý do tại sao task cần dependency đó có thể được giải thích trong Markdown.

## 22. Cấu trúc tối thiểu cho một project AI-assisted quy mô lớn

Một baseline gọn nhưng đủ mạnh có thể gồm:

```text
AGENTS.md                      # execution protocol chung, ngắn
README.md                      # project overview

/docs
  /business                    # goal, capability, flow, rule
  /requirements                # canonical requirements
  /design                      # design docs + design products
  /execution
    roadmap.yaml
    milestones.yaml
    tasklist.yaml
    /tasks                     # detailed task manifests
  /traceability
    artifacts.yaml             # deliverable registry
    relations.yaml             # graph relations nếu tách riêng
  /verification
    acceptance.yaml
    quality-gates.md

/src                           # implementation
/tests                         # executable verification
```

Cấu trúc này không nhằm tạo thêm bureaucracy. Ngược lại, mục tiêu là làm cho AI không phải đọc toàn bộ repository để đoán context. Task ID sẽ dẫn tới một context bundle nhỏ, chính xác và có thể kiểm chứng.

## 23. Nguyên tắc tổng kết

Một dự án lớn không nên được quản trị bằng “càng nhiều Markdown càng tốt”. Tài liệu chỉ hữu ích khi mỗi lớp có responsibility rõ và các object quan trọng được định danh, version và trace. Requirement định nghĩa nhu cầu; design product định nghĩa contract; deliverable định nghĩa sản phẩm hệ thống cần tồn tại; task định nghĩa đơn vị thực thi; roadmap và dependency định nghĩa thứ tự triển khai; verification định nghĩa bằng chứng.

Tasklist không phải một TODO list mà là projection của design và deliverable inventory sang execution space. Roadmap không phải một bảng ngày tháng mà là cấu trúc milestone/outcome dựa trên dependency và business priority. AI agent không nên được giao “hãy làm chức năng X” cùng một prompt dài; nó nên được giao một Task ID mà từ đó hệ thống resolve được readSet, writeSet, verifySet, dependency và baseline.

Nếu mô hình này được implement đúng, ta có thể hỏi repository những câu có tính quản trị thực sự: “Requirement này đã được implement đầy đủ chưa?”, “Màn hình này tồn tại vì requirement nào?”, “Task này được phép sửa những gì?”, “Nếu event contract này đổi thì task/test nào phải chạy lại?”, “Những task nào có thể chạy song song?”, “Milestone này còn thiếu deliverable nào?”, “Có artifact nào AI tự tạo mà không có requirement không?”. Khi repository trả lời được các câu hỏi đó bằng dữ liệu có cấu trúc thay vì bằng trí nhớ của con người hoặc khả năng đọc Markdown của AI, project mới thực sự có traceability.

> **Traceability hoàn chỉnh không dừng ở Requirement → Design → Code. Nó phải đi xuyên suốt Requirement → Design Product → Deliverable → Task → Dependency/Roadmap → Implementation → Verification → Baseline, và phải truy ngược lại được theo chiều ngược lại.**
