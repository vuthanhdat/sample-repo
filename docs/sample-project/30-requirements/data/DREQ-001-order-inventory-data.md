---
id: DREQ-001
type: DataRequirement
status: Baseline
version: 1
title: Order and inventory data requirements
relations:
  derivedFrom:
    - REQ-ORD-001
    - REQ-ORD-003
    - REQ-INV-001
  satisfiedBy:
    - DBD-001
    - DBD-002
---

# DREQ-001 — Order & Inventory Data Requirements

## Ownership

- Order Management owns `Order`, `OrderLine`, `OrderStatusHistory`.
- Inventory owns `StockBalance`, `StockReservation`, `InventoryTransaction`.
- Cross-module reference dùng stable business/system ID; module không trực tiếp sửa table thuộc owner khác.

## History and audit

- Order status transition history phải được giữ tối thiểu 7 năm trong sample policy.
- Inventory transaction/reservation history phải đủ để reconcile quantity.
- Business history không được thay thế bằng application log.

## Integrity

1. Order phải có ít nhất một valid Order Line.
2. Quantity phải > 0.
3. Monetary amount dùng decimal precision xác định, không dùng floating point.
4. Stock reservation quantity phải > 0 và không làm available stock âm.
5. Business identifiers quan trọng phải có unique constraint phù hợp.
6. Foreign key/consistency constraint chỉ dùng trong cùng ownership boundary; cross-system reference được validate qua contract/reconciliation.

## Retention and lifecycle

| Data | Retention baseline | Deletion/Archive rule |
|---|---|---|
| Order | 7 years | Archive after operational window |
| Order Status History | 7 years | Append-oriented |
| Stock Reservation | 3 years after closed | Archive allowed |
| Inventory Transaction | 7 years | Immutable business history |
| Transient idempotency keys | 30 days minimum | Purge by maintenance job |

## Migration requirement

Schema change phải backward-compatible trong rolling deployment window hoặc có coordinated deployment plan. Destructive migration cần explicit change/rollback strategy.
