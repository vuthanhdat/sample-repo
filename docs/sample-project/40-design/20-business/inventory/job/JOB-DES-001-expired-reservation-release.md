---
id: JOB-DES-001
type: DesignSpecification
subtype: BatchJob
status: Baseline
version: 1
title: Release expired stock reservations job
relations:
  specifies:
    - JOB-INV-001
  uses:
    - DB-INV-001
    - API-INV-001
  constrainedBy:
    - NFR-REL-001
    - NFR-PERF-001
---

# JOB-DES-001 — Release Expired Stock Reservations

## Purpose

Giải phóng các reservation đã quá `ExpiresAt` nhưng chưa được consume/release do Order flow bị bỏ dở hoặc failure ở upstream process.

## Trigger / schedule

- Scheduler chạy mỗi 1 phút ở baseline.
- Không yêu cầu exactly-once scheduler execution; business operation phải idempotent.

## Selection

Chọn reservation:

```text
Status = Active
AND ExpiresAt <= now
```

Processing phải theo batch/page, không load toàn bộ candidate vào memory.

## Transaction boundary

Mỗi reservation hoặc một bounded chunk được xử lý trong transaction phù hợp. Job gọi canonical Inventory application operation `ExpireReservation`; không update business table trực tiếp bằng ad-hoc scheduler SQL nếu bypass invariant.

## Concurrency

Job có thể race với fulfillment/cancel flow. State/version check phải đảm bảo chỉ một valid terminal transition thắng. Duplicate worker không được cộng stock hai lần.

## Retry/error policy

- Retry transient DB/dependency error với bounded backoff.
- Business conflict/stale reservation được classify và skip/reload thay vì blind retry.
- Sau retry limit, item được ghi failure record/metric để operator thấy và rerun.

## Rerun policy

Toàn job có thể rerun an toàn; processed reservation không bị double-release.

## Observability

Metrics:

- candidates selected;
- processed/succeeded/skipped/failed;
- processing latency;
- oldest overdue reservation age;
- retry count.

Logs phải có JobRun ID và Reservation ID.

## Verification

Test duplicate run, concurrent consume/release, partial failure, restart giữa batch và large candidate set.
