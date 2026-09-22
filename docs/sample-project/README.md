# Sample Project — Order & Inventory Management

Đây là một **bộ tài liệu mẫu có cấu trúc của một software project**, dùng để kiểm tra mô hình document/template/traceability mà SaaS sẽ quản lý. Sample không nhằm chứng minh “càng nhiều Markdown càng tốt”; nó minh họa một **Document Product Catalog** có applicability rule, ID, version, relation và coverage.

## 1. Cấu trúc đầy đủ

```text
docs/sample-project/
├── README.md
│
├── 00-governance/
│   └── DOCUMENT-CATALOG.md
│
├── 10-goals/
│   ├── GOAL-001-reduce-order-processing-time.md
│   ├── GOAL-002-improve-inventory-accuracy.md
│   └── GOAL-003-improve-order-traceability.md
│
├── 15-business-context/
│   └── BUSINESS-CONTEXT.md
│
├── 20-business-flows/
│   ├── BF-001-order-to-cash.md
│   ├── BF-002-inventory-replenishment.md
│   └── BF-003-order-cancellation-refund.md
│
├── 30-requirements/
│   ├── REQ-ORD-001-create-order.md
│   ├── REQ-ORD-002-approve-high-value-order.md
│   ├── REQ-ORD-003-track-order-status.md
│   ├── REQ-INV-001-reserve-stock.md
│   ├── non-functional/
│   │   ├── NFR-CHECKLIST.md
│   │   ├── NFR-PERF-001-performance-scalability.md
│   │   ├── NFR-REL-001-availability-reliability.md
│   │   ├── NFR-OPS-001-observability-operability.md
│   │   ├── NFR-UX-001-usability-accessibility.md
│   │   └── NFR-MNT-001-maintainability.md
│   ├── data/
│   │   └── DREQ-001-order-inventory-data.md
│   ├── integration/
│   │   └── IREQ-001-shipping-integration.md
│   └── security/
│       └── SREQ-001-auth-authorization-audit.md
│
├── 40-design/
│   ├── DES-ORD-001-order-approval-solution.md
│   ├── DES-INV-001-stock-reservation-solution.md
│   ├── architecture/
│   │   ├── ARCH-001-system-context.md
│   │   └── ARCH-002-component-boundaries.md
│   ├── data/
│   │   ├── DBD-001-logical-data-model.md
│   │   └── DBD-002-physical-schema.md
│   ├── api/
│   │   ├── API-DES-001-order-command-api.md
│   │   └── API-DES-002-stock-reservation-api.md
│   ├── screen/
│   │   └── SCR-DES-001-order-detail.md
│   ├── job/
│   │   └── JOB-DES-001-expired-reservation-release.md
│   ├── event/
│   │   └── EVT-DES-001-order-status-changed.md
│   ├── integration/
│   │   └── INT-DES-001-shipping-provider.md
│   ├── security/
│   │   └── SEC-DES-001-authorization-audit.md
│   └── configuration/
│       └── CFG-DES-001-business-configuration.md
│
├── 50-deliverables/
│   ├── API-ORD-001-order-command-api.md
│   ├── API-INV-001-stock-reservation-api.md
│   ├── SCR-ORD-001-order-detail.md
│   ├── EVT-ORD-001-order-status-changed.md
│   ├── DB-ORD-001-order-database.md
│   ├── DB-INV-001-inventory-database.md
│   ├── JOB-INV-001-release-expired-reservations.md
│   └── INT-SHP-001-shipping-interface.md
│
├── 55-planning/
│   └── ROADMAP-001-delivery-plan.md
│
├── 60-tasks/
│   ├── TASK-DATA-001-implement-database-foundation.md
│   ├── TASK-ORD-BE-001-implement-order-api.md
│   ├── TASK-INV-BE-001-implement-stock-reservation.md
│   ├── TASK-ORD-FE-001-implement-order-detail.md
│   ├── TASK-JOB-001-implement-expiry-job.md
│   ├── TASK-INT-001-implement-shipping-adapter.md
│   ├── TASK-E2E-001-order-flow-test.md
│   └── TASK-OPS-001-production-readiness.md
│
├── 70-verification/
│   ├── TEST-STRATEGY.md
│   ├── TEST-ORD-001-order-approval.md
│   ├── TEST-INV-001-stock-reservation.md
│   ├── TEST-SHP-001-shipping-contract.md
│   ├── PERF-001-order-inventory-load.md
│   ├── REL-TEST-001-reliability-idempotency.md
│   └── SEC-TEST-001-authorization-audit.md
│
├── 80-operations/
│   ├── DEP-001-deployment-design.md
│   ├── OBS-001-observability-design.md
│   └── RUN-001-order-inventory-runbook.md
│
├── 85-change-management/
│   └── CR-001-change-approval-threshold.md
│
└── 90-traceability/
    ├── DOCUMENT-COVERAGE.md
    └── TRACEABILITY.md
```

## 2. Ý nghĩa từng layer

| Layer | Câu hỏi phải trả lời |
|---|---|
| Governance | Project template yêu cầu những document/design products nào? |
| Goal / Business Context | Vì sao project tồn tại, actor/capability/domain là gì? |
| Business Flow | Nghiệp vụ chạy qua những bước/actor/system nào? |
| Requirements | Điều gì **phải đúng** — functional và non-functional? |
| Design Decision | Ta chọn hình dạng solution nào và vì sao? |
| Design Specification | API/DB/screen/job/event/interface cụ thể phải tuân contract gì? |
| Deliverable | Những output nào cuối cùng **phải tồn tại** trong hệ thống? |
| Planning / Task | Công việc nào tạo/thay đổi các output đó, dependency ra sao? |
| Verification | Bằng chứng nào chứng minh requirement/output đạt chuẩn? |
| Operations | Deploy, quan sát, support và recovery hệ thống như thế nào? |
| Change Management | Baseline thay đổi ra sao và ảnh hưởng object nào? |
| Traceability | Chuỗi trên có coverage và truy ngược được không? |

## 3. Requirement không chỉ là Functional Requirement

Mô hình sample coi requirement là nhiều dimension song song:

```text
Business Goal / Flow
        ↓
Functional Requirements ───────────┐
Non-functional Requirements ───────┤
Data Requirements ─────────────────┤
Integration Requirements ──────────┤──> Architecture / Design
Security Requirements ─────────────┘
```

`NFR-PERF-001` chẳng hạn không nằm “dưới” một API duy nhất; nó constrain API, DB, screen và job cùng lúc. Vì vậy folder chỉ là navigation, còn graph mới là model thật.

`NFR-CHECKLIST.md` buộc project review các category như performance, scalability, reliability, DR, security, privacy, operability, usability/accessibility, maintainability, interoperability, compliance... rồi đánh dấu `Applicable` hoặc `Not Applicable` có lý do.

## 4. Design Decision khác Design Specification

Ví dụ:

```text
REQ-INV-001
     ↓ satisfied-by
DES-INV-001                    ← WHY / solution decision
     ├── introduces API-INV-001
     ├── introduces DB-INV-001
     └── introduces JOB-INV-001

API-DES-002 ── specifies ──> API-INV-001
DBD-001/002 ── specifies ──> DB-INV-001
JOB-DES-001 ── specifies ──> JOB-INV-001
```

Như vậy “có một design document” không đủ. Project phải biết **design products nào bắt buộc theo loại deliverable**. Job cần schedule/selection/transaction/retry/rerun/concurrency/observability; DB cần ownership/logical/physical schema/index/migration/retention; Event cần schema/version/delivery/idempotency; mỗi loại có checklist khác nhau.

## 5. Deliverable là inventory output, không phải file design

Ví dụ:

```text
DBD-002                           DB-INV-001
Physical DB Design   specifies   Inventory DB Deliverable
                                  ↓ realized-by
                                  migration/source artifacts

JOB-DES-001                        JOB-INV-001
Job Specification     specifies   Runtime Job Deliverable
```

Design Specification mô tả output; Deliverable là thứ project yêu cầu hệ thống thực tế phải có; source/migration/config/deployment files là Implementation Artifacts hiện thực deliverable đó.

## 6. Task là execution contract

Task không phải một TODO title. Ví dụ `TASK-INV-BE-001` có graph context:

```text
reads
  REQ-INV-001 + BR/AC
  NFR-PERF / NFR-REL / NFR-OPS / SREQ / DREQ
  DES-INV-001
  ARCH-001 / ARCH-002
  API-DES-002
  DBD-001 / DBD-002

implements
  API-INV-001

depends-on
  TASK-DATA-001

verified-by
  TEST-INV-001
  PERF-001
  REL-TEST-001
  SEC-TEST-001
```

Human hay AI nhận Task ID đều phải resolve đúng context này; không tự crawl toàn repo và đoán requirement.

## 7. Verification bao gồm NFR

Functional E2E pass chưa đủ để Deliverable được Verified. Sample có:

- functional/integration verification;
- shipping contract verification;
- performance/load verification;
- concurrency/idempotency/reliability verification;
- authorization/audit verification;
- architecture/static quality gate theo `TEST-STRAT-001`;
- operational readiness evidence.

Verification Definition và Verification Run/Evidence vẫn là hai khái niệm khác nhau.

## 8. Operations cũng thuộc project knowledge

`DEP-001`, `OBS-001`, `RUN-001` mô tả cách deploy, quan sát, alert, khôi phục và support production. Một system có code/test đẹp nhưng không thể chẩn đoán hoặc restore khi failure thì chưa đủ production-ready.

## 9. Change Management

`CR-001` minh họa thay đổi approval threshold. Hệ thống không chỉ sửa `BR-ORD-001`; nó traverse graph để xác định candidate impacts tới requirement, configuration design, API, screen và tests, sau đó từng impact được disposition:

```text
Update Required
Review Required
Revalidation Required
Replan Required
No Change Required
```

Đây là cơ chế để trả lời câu hỏi “thay đổi này phải update tài liệu/deliverable/test nào?”.

## 10. ProjectTemplate không tạo bureaucracy

`DOCUMENT-CATALOG.md` là checklist có điều kiện. Nếu project không có batch/job, `Job Design` được đánh dấu `Not Applicable` có reason; không cần sinh file rỗng. Nếu sau này scope thêm background processing, item chuyển sang Applicable và app biết design/output/verification coverage nào phải có.

Mục tiêu của SaaS là quản lý trạng thái này bằng structured data:

```text
Not Evaluated
   ├─> Applicable → Draft → Baseline
   └─> Not Applicable + reason
```

## 11. Derived views

- `90-traceability/TRACEABILITY.md`: graph/matrix Goal → Requirement/NFR → Design → Deliverable → Task → Verification → Operations/Change.
- `90-traceability/DOCUMENT-COVERAGE.md`: checklist những document/design products nào đã đủ, thiếu hoặc Not Applicable.

Hai file này trong app thật nên được **generate**, không nhập tay làm source of truth.
