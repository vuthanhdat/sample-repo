# Design Structure

Design được tách theo scope và decision area.

```text
40-design/
├── 10-common/      # technical baseline toàn application / nhiều feature
└── 20-feature/     # design riêng từng FEATURE-*
```

Phân loại một design bằng câu hỏi: **nếu thay toàn bộ nghiệp vụ bằng một feature khác, design này còn đúng không?** Nếu còn đúng thì thuộc `10-common`; nếu không thì thuộc `20-feature/<feature-id>/`.

## Common design areas

```text
10-common/
├── architecture/
├── backend/
├── frontend/
├── api/
├── integration/
├── data/
├── security/
├── infrastructure/
└── cross-cutting/
```

Các vùng lớn phải được breakdown khi chúng chứa nhiều quyết định có lifecycle độc lập. Ví dụ:

- `architecture/`: system structure, module/application structure, runtime interaction.
- `backend/`: backend architecture, persistence, transaction/concurrency, background processing.
- `frontend/`: frontend architecture, UI/UX standards, state/data flow, navigation/routing.
- `integration/`: sync/async/event/file interaction patterns và contract/resilience rules.
- `data/`: modeling, persistence, lifecycle/history, migration.
- `infrastructure/`: deployment topology, compute/runtime, network, storage, environment topology.
- `cross-cutting/`: error handling, observability/logging, configuration, resilience, caching, localization, audit, feature flags khi applicable.

## Feature design areas

Trong feature branch, level tiếp theo là artifact/decision type. Allowed taxonomy gồm:

```text
feature-NNN/
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

Không tạo folder hoặc document chỉ để đủ taxonomy. Feature chỉ materialize những loại output/design thực sự applicable.

## Design decomposition rule

Một design document nên được tách nhỏ hơn khi có một trong các dấu hiệu:

1. Chứa nhiều quyết định có thể thay đổi độc lập.
2. Có nhiều owner/reviewer khác nhau.
3. Một phần có thể áp dụng cho nhiều feature nhưng phần khác không thể.
4. Impact của thay đổi chỉ chạm một phần document.
5. Document bắt đầu trộn logical architecture, runtime, infrastructure, data hoặc operations trong cùng một nơi.

Folder là navigation projection; metadata `scope`, `feature`, `type`, ID và relations mới là model thật.