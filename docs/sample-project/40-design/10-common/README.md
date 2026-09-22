# Common / Application Design

Common Design là technical baseline áp dụng cho toàn application hoặc nhiều feature. Nó **không chứa entity/rule/state của một feature cụ thể**.

Các feature design mặc định kế thừa common baseline. Chỉ override khi có exception/decision được quản lý.

```text
Common Design
   ↓ applies-to
Feature Design
   ↓ specifies
Deliverable
   ↓ implemented-by
Task
```

## Decision areas

```text
10-common/
├── architecture/       # system/module/runtime structure
├── backend/            # backend architecture and processing conventions
├── frontend/           # frontend architecture and UI interaction conventions
├── api/                # API protocol/contract conventions
├── integration/        # sync/async/event/file integration conventions
├── data/               # data model/persistence/lifecycle/migration conventions
├── security/           # authentication, authorization and security baseline
├── infrastructure/     # deployment/runtime/network/storage topology
└── cross-cutting/      # concerns spanning multiple layers/features
```

Mỗi area có thể tiếp tục breakdown thêm một level khi project đủ lớn. Không cần tạo mọi subfolder ngay từ đầu; README của từng area mô tả các decision types có thể materialize khi applicable.

## Boundary with Operations

Common Design trả lời **solution được thiết kế như thế nào**. `80-operations/` trả lời **solution được deploy, configure, observe, recover và release như thế nào trong môi trường thực tế**. Ví dụ deployment topology thuộc infrastructure design; deployment procedure thuộc operations.

Common changes có blast radius lớn và phải impact-analysis theo scope.