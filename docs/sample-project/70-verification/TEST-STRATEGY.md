---
id: TEST-STRAT-001
type: TestStrategy
status: Baseline
version: 1
title: Order and Inventory verification strategy
---

# TEST-STRAT-001 — Verification Strategy

## 1. Purpose

Verification không chỉ chứng minh happy path chạy được. Strategy phải cover business behavior, contracts, data integrity, architecture boundaries và các Non-functional Requirement quan trọng.

## 2. Verification layers

| Layer | Mục tiêu | Ví dụ |
|---|---|---|
| Unit | Domain calculation/state/rule | approval/state transitions |
| Integration | DB, transaction, adapter | reservation concurrency, migrations |
| Contract | API/event/external contract | OpenAPI/event schema/shipping adapter |
| E2E | Business flow xuyên component | `BF-001` |
| Architecture | Dependency/boundary | `ARCH-002` rules |
| Performance | NFR latency/load | `PERF-001` |
| Reliability | retry/idempotency/recovery | `REL-TEST-001` |
| Security | permission/audit | `SEC-TEST-001` |
| Operational | log/metric/runbook/deploy | operational readiness review |

## 3. Traceability rule

Mỗi baseline Requirement/NFR áp dụng phải có ít nhất một verification definition hoặc một approved rationale rằng requirement được verify ở level khác. Deliverable `Verified` yêu cầu successful verification run trên revision phù hợp, không chỉ có file test definition.

## 4. Test data

- Stable seeded data cho deterministic tests.
- Performance dataset có scale profile rõ.
- Security tests dùng identities/roles riêng.
- External provider test dùng sandbox/stub/contract fixture, không phụ thuộc production endpoint.

## 5. Quality gates

Baseline release gate:

```text
Build
→ Static / Architecture checks
→ Unit
→ Integration + migration
→ Contract
→ Critical E2E
→ Security checks
→ Required NFR verification
```

Một expensive test như full performance suite có thể chạy release/nightly thay vì mọi commit, nhưng release evidence phải gắn target revision.

## 6. Failure evidence

Failure result phải chứa test ID, target revision, environment và link tới log/artifact đủ để xác định object nào không đạt. Một câu "tests failed" không đủ làm verification evidence.
