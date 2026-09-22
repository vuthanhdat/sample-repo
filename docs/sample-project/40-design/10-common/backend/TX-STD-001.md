---
id: TX-STD-001
type: CommonStandard
scope: Backend
status: Draft
version: 1
---
# TX-STD-001 — Transaction & Concurrency Standard

- Mỗi write use case phải có transaction boundary rõ.
- Không giữ DB transaction trong lúc chờ external network call nếu không có lý do đặc biệt.
- Chọn optimistic/pessimistic concurrency theo conflict model và document trong feature design.
- Retry chỉ cho transient failure và operation phải idempotent hoặc có deduplication.
- Job/batch phải định nghĩa unit-of-work/chunk boundary, rerun semantics và partial-failure policy.
- Outbox/inbox hoặc equivalent được dùng khi cần consistency giữa DB và messaging.