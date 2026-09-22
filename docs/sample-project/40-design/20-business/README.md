# Business / Feature Design

`20-business/` chứa design xuất phát từ business requirement/domain. Cấu trúc được tổ chức **theo domain trước**, sau đó mới theo loại artifact.

```text
20-business/
├── README.md
├── 00-shared-domain/
├── order/
├── inventory/
└── shipping/
```

## Why domain-first

Cùng một feature thường cần nhiều output cùng lúc: API, screen, event, job, configuration, data. Nếu root chỉ chia `api/`, `screen/`, `job/` thì knowledge của một business feature bị rải khắp nơi. Domain-first giữ các design liên quan gần nhau, nhưng vẫn cho phép app tạo view theo artifact type từ metadata/deliverable registry.

## Boundary rule

- `00-shared-domain`: design nghiệp vụ dùng chung nhiều bounded context.
- `order`: chỉ design thuộc Order capability.
- `inventory`: chỉ design thuộc Inventory capability.
- `shipping`: chỉ design thuộc Shipping integration/capability.

Business Design luôn phải trace về Requirement/Rule/NFR và trace xuống Deliverable. Common technical standards được kế thừa từ `../10-common/`, không copy vào từng file.

## Example

```text
REQ-ORD-002
   ↓ satisfied-by
DES-ORD-001
   ├── API-DES-001
   ├── SCR-DES-001
   └── EVT-DES-001

Common baseline inherited:
APP-ARCH-001 + BE-ARCH-001 + FE-ARCH-001 + API-STD-001 + TX-STD-001 + AUTH-DES-001
```