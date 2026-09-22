---
id: TX-STD-001
type: CommonDesign
scope: Common
status: Baseline
version: 1
title: Transaction and Concurrency Rules
appliesTo:
  - Backend
  - Database
  - Job
---

# TX-STD-001 — Transaction and Concurrency Rules

## Default transaction boundary

Một application use case thay đổi business state phải có transaction boundary rõ ràng. Mặc định transaction bao quanh các write cần atomicity trong cùng database boundary.

## External I/O

Không giữ database transaction mở trong lúc gọi external HTTP/service nếu có thể tránh. Coordination giữa DB state và message/event sử dụng outbox/inbox hoặc workflow phù hợp khi cần đảm bảo delivery.

## Concurrency

- Optimistic concurrency là default cho aggregate có conflict hiếm.
- Pessimistic locking chỉ dùng khi invariant yêu cầu và phải giới hạn lock scope/time.
- Stock/counter/financial-like updates phải có explicit concurrency strategy.

## Retry

Retry chỉ áp dụng cho lỗi transient đã phân loại. Retry toàn use case phải idempotent hoặc có deduplication mechanism.

## Job transaction

Background job không mặc định xử lý toàn batch trong một transaction lớn. Phải định nghĩa unit-of-work/chunk boundary, retry và resume behavior.

## Isolation and constraints

Isolation level phải đủ bảo vệ invariant nhưng tránh nâng toàn hệ thống lên mức cao nhất không cần thiết. Unique/check/FK constraints được dùng như integrity guard tương ứng với logical model.

## Design requirement

Business design nào có transaction semantics khác baseline phải ghi rõ boundary, failure mode, compensation và concurrency strategy.