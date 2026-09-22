# Document Catalog

Catalog này định nghĩa bộ knowledge/work products tổng quát cho một software project. Nội dung trong `sample-project` chỉ dùng ID generic như `FEATURE-001`, `REQ-F001-001`; không gắn với domain cụ thể.

## Lifecycle layers

| Layer | Mục đích | Output chính |
|---|---|---|
| Governance | Quy tắc tài liệu, ID, relation | catalog, ID convention, relation vocabulary |
| Goals | Kết quả project muốn đạt | `GOAL-*` |
| Context | actor, glossary, boundary | context, actor list, glossary |
| Business Flow | flow nghiệp vụ mức cao | `BF-*` |
| Feature | capability/feature project phải cung cấp | `FEATURE-*` |
| Requirements | WHAT + constraint | `REQ-*`, `NFR-*`, `DREQ-*`, `IREQ-*`, `SREQ-*` |
| Design | HOW | Common Design + Feature Design |
| Deliverables | output hữu hình của solution | screen/API/job/event/DB/interface/file/mail/... |
| Planning | roadmap, milestone | `ROADMAP-*`, `MS-*` |
| Tasks | execution unit | `TASK-*` |
| Verification | definition/evidence of correctness | `TEST-*`, `PERF-*`, ... |
| Operations | deploy/observe/run | `DEP-*`, `OBS-*`, `RUN-*` |
| Change | baseline changes | `CR-*` |
| Traceability | coverage/impact views | derived views |

## Design taxonomy

```text
40-design/
├── 10-common/      # áp dụng toàn application / nhiều feature
└── 20-feature/     # design riêng từng feature
    ├── feature-001/
    └── feature-002/
```

Common Design không được chứa rule/entity/state của một feature cụ thể. Feature Design kế thừa Common Design và chỉ mô tả phần khác biệt của feature.

## Deliverable inventory

Mỗi output type có list tổng hợp tại `50-deliverables/00-lists/`. List là management view, còn detailed specification nằm trong `40-design/20-feature/...`.

Các loại chuẩn gồm: Screen, API, Batch/Job, Interface, Event, Database, File, Mail, Notification, Report.

## Applicability

Không phải project nào cũng có mọi loại output. ProjectTemplate quản lý:

```text
Not Evaluated
├── Applicable → Draft → Baseline
└── Not Applicable + reason
```

Không tạo file rỗng chỉ để đủ checklist.