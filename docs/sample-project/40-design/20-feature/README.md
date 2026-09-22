# Feature Design

Feature Design chứa HOW cho từng `FEATURE-*`. Cấu trúc vật lý là:

```text
20-feature/
├── feature-001/
└── feature-002/
```

Mỗi feature chỉ materialize những decision area applicable. Allowed taxonomy:

```text
feature-NNN/
├── decision/            # feature-specific trade-off/exception/ADR-like decisions
├── domain/              # domain model, aggregate/entity/value object, invariants
├── workflow-state/      # state machine, lifecycle, orchestration within feature
├── screen/              # screen/form/list interaction design
├── api/                 # endpoint/operation contract
├── job/                 # batch/background job design
├── event/               # event contract and publish/consume behavior
├── integration/         # external/internal integration design
├── data/                # feature-owned persistence/data design
├── file/                # file import/export/generated file design
├── mail-notification/   # mail/notification content and delivery behavior
└── report/              # report/output layout and data composition
```

`domain/` và `workflow-state/` là design area độc lập với database: business model/state transition không nên bị nhét vào `data/` chỉ vì cuối cùng chúng được persist.

Feature Design kế thừa Common Design ở `../10-common/` và không lặp lại API/pagination/transaction/auth/error/logging/integration/infrastructure standards chung.

Không tạo folder rỗng chỉ để đủ taxonomy; loại không applicable phải được đánh giá rõ ràng ở coverage/applicability view.