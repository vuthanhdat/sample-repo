---
id: EVT-DES-001
type: DesignSpecification
subtype: Event
status: Baseline
version: 1
title: OrderStatusChanged event contract
relations:
  specifies:
    - EVT-ORD-001
  satisfies:
    - REQ-ORD-003
  constrainedBy:
    - NFR-REL-001
---

# EVT-DES-001 — OrderStatusChanged Event Contract

## Semantic meaning

Event biểu thị rằng một valid Order state transition đã được committed. Consumer không dùng event này để tự suy luận transition chưa xảy ra trong Order owner.

## Contract

```json
{
  "eventId": "uuid",
  "eventType": "OrderStatusChanged",
  "schemaVersion": 1,
  "orderId": "uuid",
  "previousStatus": "PendingApproval",
  "newStatus": "Approved",
  "occurredAt": "2026-09-22T03:00:00Z",
  "actorId": "...",
  "correlationId": "...",
  "causationId": "..."
}
```

## Publishing

Event chỉ publish cho committed business transition. Implementation nên dùng transactional outbox hoặc equivalent nếu DB commit và broker publish không thể nằm trong một atomic boundary.

## Delivery assumptions

- Delivery có thể `at-least-once`; consumer phải idempotent theo `eventId` hoặc business idempotency strategy.
- Global ordering không được giả định. Nếu ordering per Order cần thiết, consumer dùng order/version/occurred metadata theo contract.
- Breaking schema change tạo new schema version và compatibility plan.

## Failure handling

Publisher failure sau DB commit phải recover qua outbox/retry, không rollback giả tạo business transaction đã committed.

## Verification

Contract/schema validation, duplicate delivery, delayed delivery và publisher restart phải được test.
