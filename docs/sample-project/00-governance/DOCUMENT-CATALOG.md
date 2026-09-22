# Document Catalog

Catalog này định nghĩa bộ knowledge/work products tổng quát cho một software project. Nội dung trong `sample-project` chỉ dùng ID generic như `FEATURE-001`, `REQ-F001-001`; không gắn với domain cụ thể.

Nguyên tắc chính: **document model phải phản ánh các decision area của software lifecycle**. Một document hoặc folder quá lớn cần được breakdown khi nó chứa nhiều quyết định có thể thay đổi, review hoặc impact độc lập.

## Lifecycle layers

| Layer | Mục đích | Output chính |
|---|---|---|
| Governance | Quy tắc tài liệu, ID, relation | catalog, ID convention, relation vocabulary |
| Goals | Kết quả project muốn đạt | `GOAL-*` |
| Context | boundary, scope, actor, external systems, assumptions, terminology | context/scope/actor/external-system/glossary products |
| Business Flow | flow nghiệp vụ mức cao | `BF-*` |
| Feature | capability/feature project phải cung cấp | `FEATURE-*` |
| Requirements | WHAT + rule + constraint | `REQ-*`, `BR-*`, `NFR-*`, `DREQ-*`, `IREQ-*`, `SREQ-*` |
| Design | HOW | Common Design + Feature Design |
| Deliverables | output hữu hình của solution | screen/API/job/event/DB/interface/file/mail/... |
| Planning | roadmap, milestone | `ROADMAP-*`, `MS-*` |
| Tasks | execution unit | `TASK-*` |
| Verification | definition/evidence of correctness | `TEST-*`, `PERF-*`, ... |
| Operations | deploy/configure/observe/recover/run/release | deployment/environment/observability/backup/runbook/release products |
| Change | baseline changes | `CR-*` |
| Traceability | coverage/impact views | derived views |

## Context taxonomy

`15-context/` nên tách các loại knowledge khác lifecycle:

- Project context: mục tiêu, background và environment chung.
- Scope / boundary: in-scope, out-of-scope, system boundary.
- Actors: human/system roles tương tác với solution.
- External systems: dependency/system bên ngoài boundary.
- Assumptions / constraints: business, legacy, technology, regulatory assumptions và constraints.
- Glossary: ubiquitous language và thuật ngữ.

## Requirement taxonomy

```text
30-requirements/
├── functional/       # behavior/capability cụ thể
├── business-rules/   # rule/policy dùng chung hoặc chi phối nhiều requirement
├── data/             # data ownership, retention, quality, history...
├── integration/      # external/internal interaction requirement
├── non-functional/   # quality attributes
└── security/         # security/privacy-specific requirement
```

Business rule có thể nằm trực tiếp trong functional requirement nếu local và đơn giản. Khi một rule được reuse hoặc govern nhiều requirement/feature thì tách thành `BR-*` riêng.

## Design taxonomy

```text
40-design/
├── 10-common/
│   ├── architecture/
│   ├── backend/
│   ├── frontend/
│   ├── api/
│   ├── integration/
│   ├── data/
│   ├── security/
│   ├── infrastructure/
│   └── cross-cutting/
└── 20-feature/
    └── feature-NNN/
        ├── decision/
        ├── screen/
        ├── api/
        ├── job/
        ├── event/
        ├── integration/
        ├── data/
        ├── file/
        ├── mail-notification/
        └── report/
```

Common Design không được chứa rule/entity/state của một feature cụ thể. Feature Design kế thừa Common Design và chỉ mô tả phần khác biệt của feature.

Các vùng Common Design lớn có thể breakdown thêm:

- Architecture: system, module/application, runtime views.
- Backend: architecture, persistence, transaction/concurrency, background processing.
- Frontend: architecture, UI/UX, state/data flow, navigation/routing.
- Data: modeling, persistence, lifecycle/history, migration.
- Integration: synchronous, asynchronous/event, file/batch, resilience/contract policies.
- Infrastructure: deployment topology, compute/runtime, network, storage, environment topology.
- Cross-cutting: error, observability/logging, configuration, resilience, caching, localization, auditing, feature flags as applicable.

Không phải mọi project cần materialize mọi folder. Taxonomy định nghĩa **allowed decision areas**, còn applicability quyết định document nào phải tồn tại.

## Deliverable inventory

Mỗi output type có list tổng hợp tại `50-deliverables/00-lists/`. List là management view, còn detailed specification nằm trong `40-design/20-feature/...`.

Các loại chuẩn gồm: Screen, API, Batch/Job, Interface, Event, Database, File, Mail, Notification, Report.

Feature design taxonomy nên cho phép specification tương ứng với deliverable taxonomy, nhưng chỉ tạo loại applicable cho feature đó.

## Operations taxonomy

```text
80-operations/
├── deployment/
├── environments/
├── observability/
├── backup-recovery/
├── runbooks/
└── release/
```

Phân biệt rõ **design** và **operations**: infrastructure/deployment architecture mô tả HOW solution được bố trí; operations mô tả cách deploy, configure, observe, recover và release solution trong thực tế.

## Applicability

Không phải project nào cũng có mọi loại requirement/design/output. ProjectTemplate quản lý:

```text
Not Evaluated
├── Applicable → Draft → Baseline
└── Not Applicable + reason
```

Không tạo file rỗng chỉ để đủ checklist.