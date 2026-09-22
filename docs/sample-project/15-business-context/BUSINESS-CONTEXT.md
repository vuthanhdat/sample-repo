---
documentId: DOC-BIZ-CONTEXT-001
documentType: BusinessContext
status: Baseline
version: 1
objects:
  - CAP-ORD-001
  - CAP-INV-001
  - ACT-CUSTOMER
  - ACT-OPS
  - ACT-WAREHOUSE
---

# Business Context — Order & Inventory Management

## 1. Scope

Sample project quản lý vòng đời Order và lượng tồn kho có thể cam kết cho Order. Phạm vi chính gồm Order Management và Inventory Reservation; payment, warehouse execution và shipping được xem là external/adjacent context.

## 2. Business capabilities

### CAP-ORD-001 — Order Management

Khả năng tạo Order, áp dụng approval policy, quản lý state transition, cancellation và truy vết lịch sử Order.

### CAP-INV-001 — Inventory Availability & Reservation

Khả năng biết lượng stock khả dụng, reserve stock cho Order và release reservation an toàn.

## 3. Actors

| ID | Actor | Responsibility |
|---|---|---|
| `ACT-CUSTOMER` | Customer | Tạo/yêu cầu hủy đơn, xem trạng thái |
| `ACT-OPS` | Operations Staff | Theo dõi, approve/cancel theo quyền, xử lý exception |
| `ACT-WAREHOUSE` | Warehouse Staff | Ghi nhận nhập/điều chỉnh tồn kho |
| `ACT-SYSTEM` | Automated Process | Validation, reservation, timeout/release, event publishing |

## 4. System boundaries

```text
Customer / Operations
        ↓
Order Management
        ↕
Inventory
        ↕
External Shipping / Payment / Notification
```

Order Management sở hữu Order state. Inventory sở hữu stock balance/reservation. External providers không được trở thành source of truth cho Order hoặc Inventory nội bộ.

## 5. Ubiquitous language

| Term | Meaning |
|---|---|
| Order | Yêu cầu mua hàng của khách đã được hệ thống ghi nhận |
| Order Line | Một sản phẩm và quantity trong Order |
| Available Stock | Lượng stock còn có thể cam kết sau khi trừ reservation |
| Reservation | Cam kết tạm thời một lượng stock cho Order |
| High-value Order | Order vượt approval threshold cấu hình |
| Fulfillment Ready | Order đã thỏa điều kiện để chuyển sang xử lý giao hàng |

## 6. Related objects

- Goals: `GOAL-001`, `GOAL-002`, `GOAL-003`.
- Business flows: `BF-001`, `BF-002`, `BF-003`.
- Capabilities: `CAP-ORD-001`, `CAP-INV-001`.
