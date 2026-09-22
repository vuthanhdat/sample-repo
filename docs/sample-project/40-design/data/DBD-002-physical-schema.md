---
id: DBD-002
type: DataDesignSpecification
subtype: PhysicalSchema
status: Baseline
version: 1
title: PostgreSQL physical schema baseline
relations:
  refines:
    - DBD-001
  satisfies:
    - DREQ-001
    - NFR-PERF-001
    - NFR-REL-001
  specifies:
    - DB-ORD-001
    - DB-INV-001
---

# DBD-002 — PostgreSQL Physical Schema Baseline

## 1. Order tables

### `orders`

| Column | Type | Constraint |
|---|---|---|
| `id` | uuid | PK |
| `order_no` | varchar(40) | unique, not null |
| `customer_id` | uuid | not null |
| `status` | varchar(32) | not null |
| `total_amount` | numeric(19,4) | not null, >= 0 |
| `currency` | char(3) | not null |
| `row_version` | bigint | not null |
| `created_at` | timestamptz | not null |
| `updated_at` | timestamptz | not null |

Indexes: `(customer_id, created_at desc)`, `(status, updated_at)`.

### `order_lines`

PK `id`, FK `order_id -> orders.id`; `quantity > 0`; monetary columns use `numeric(19,4)`.

### `order_status_history`

Append-oriented table with index `(order_id, changed_at, id)` for timeline paging. Application path does not update/delete history rows.

## 2. Inventory tables

### `stock_balances`

Unique `(product_id, location_id)`. Store `on_hand_quantity`, `reserved_quantity`, `row_version`; available quantity is calculated consistently from balance fields or maintained by one canonical write path.

### `stock_reservations`

Unique `reservation_id`; unique active/idempotency constraint appropriate to `(order_id, product_id, location_id, idempotency_key)`. Index `expires_at` for expiry job and `(order_id, status)` for release/reconciliation.

### `inventory_transactions`

Append-oriented transaction ledger with reference type/id, quantity delta, occurred time and correlation metadata.

## 3. Concurrency

Reservation update uses optimistic concurrency/version check or row-level lock strategy within Inventory ownership boundary. Chosen implementation must prove `BR-INV-001` under concurrent reserve tests.

## 4. Migration rules

- Additive change trước destructive change.
- Index creation trên large production table cần online/concurrent strategy khi platform hỗ trợ.
- Migration phải có forward verification và rollback/roll-forward note.
- No application dependency on implicit DB default for business-significant values unless explicitly designed.

## 5. Retention / partition consideration

`order_status_history` và `inventory_transactions` là candidates cho time-based partition/archive khi volume threshold chứng minh cần thiết; không partition sớm chỉ vì dự đoán.
