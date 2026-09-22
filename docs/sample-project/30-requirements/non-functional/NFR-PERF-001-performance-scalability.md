---
id: NFR-PERF-001
type: NonFunctionalRequirement
category: PerformanceScalability
status: Baseline
version: 1
title: Order and inventory performance targets
relations:
  constrains:
    - API-ORD-001
    - API-INV-001
    - SCR-ORD-001
    - DB-ORD-001
---

# NFR-PERF-001 — Performance & Scalability

## Requirement

Hệ thống phải đáp ứng tải vận hành mục tiêu mà không làm mất tính đúng đắn của Order/Inventory transaction.

## Workload baseline

- 100 requests/second cho read traffic ở điều kiện bình thường.
- 20 order commands/second ở peak business window.
- Tối đa 500 concurrent interactive users cho sample baseline.
- Dataset thiết kế ban đầu: 10 triệu Order, 50 triệu Order Status History và 5 triệu active/historical Stock Reservation records.

## Service targets

| Operation | Target |
|---|---|
| `GET Order Detail` | P95 ≤ 500 ms |
| `POST Create Order` | P95 ≤ 800 ms |
| `POST Approve/Cancel Order` | P95 ≤ 800 ms |
| `POST Reserve Stock` | P95 ≤ 500 ms khi không có contention bất thường |
| Order Detail first usable render | P95 ≤ 2 seconds trên supported corporate network |

## Scalability rules

1. API tier phải có thể scale horizontally, không lưu session business state trong process memory.
2. Database query cho Order Detail/Status History phải có bounded pagination/index strategy.
3. Stock reservation phải ưu tiên correctness; không được giảm transaction isolation chỉ để đạt latency target mà gây oversell.
4. Background cleanup/reconciliation không được làm degradation đáng kể cho online commands.

## Verification

- `PERF-001` phải kiểm tra representative workload và percentile latency.
- Performance result phải ghi target revision, dataset size và test environment để so sánh có ý nghĩa.
