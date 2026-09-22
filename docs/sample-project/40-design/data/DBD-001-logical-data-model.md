---
id: DBD-001
type: DataDesignSpecification
subtype: LogicalModel
status: Baseline
version: 1
title: Order and inventory logical data model
relations:
  satisfies:
    - DREQ-001
  specifies:
    - DB-ORD-001
    - DB-INV-001
---

# DBD-001 — Logical Data Model

## 1. Order aggregate

```text
Order
  ├── OrderLine [1..n]
  ├── OrderStatusHistory [0..n]
  └── ApprovalDecision [0..n]
```

`Order` là aggregate root cho command/state transition. `OrderStatusHistory` là business history append-oriented, không phải bản copy mutable của Order.

### Order

- OrderId
- CustomerId
- Status
- TotalAmount
- Currency
- ShippingAddress snapshot/reference theo policy
- CreatedAt / UpdatedAt
- Version (optimistic concurrency)

### OrderLine

- OrderLineId
- OrderId
- ProductId
- Quantity
- UnitPrice
- LineAmount

### OrderStatusHistory

- HistoryId
- OrderId
- PreviousStatus
- NewStatus
- ChangedAt
- ActorId / Source
- Reason
- CorrelationId

## 2. Inventory aggregate

```text
StockBalance
  └── StockReservation [0..n]
InventoryTransaction [0..n]
```

### StockBalance

- ProductId + LocationId (business uniqueness)
- OnHandQuantity
- ReservedQuantity
- AvailableQuantity (derived or persisted with invariant)
- Version

### StockReservation

- ReservationId
- OrderId
- ProductId
- LocationId
- Quantity
- Status: Active / Released / Expired / Consumed
- IdempotencyKey
- CreatedAt / ExpiresAt / ClosedAt

### InventoryTransaction

Immutable business ledger cho receipt, adjustment, reserve/release/consume-related quantity movement cần audit.

## 3. Cross-boundary references

Order không FK trực tiếp vào Inventory internal table. `OrderId` trong StockReservation là external/business reference; consistency cross-module được duy trì qua application contract/reconciliation.

## 4. History rule

Không dùng một generic `History` table cho mọi entity. Mỗi history object phải phản ánh business semantics: Order Status History khác Inventory Transaction về meaning, retention và query pattern.
