---
id: PERF-001
type: VerificationDefinition
subtype: Performance
status: Baseline
version: 1
title: Order and Inventory load/performance verification
relations:
  verifies:
    - NFR-PERF-001
    - API-ORD-001
    - API-INV-001
    - SCR-ORD-001
---

# PERF-001 — Order & Inventory Performance Verification

## Workload profile

Run representative mixed traffic against production-like environment/data scale:

- Order Detail reads.
- Create/approve/cancel Order commands.
- Stock reservation/release commands.
- Background reservation expiry load running concurrently.

## Assertions

- Endpoint P95 targets from `NFR-PERF-001` are met.
- Error rate remains under defined test threshold and no correctness invariant fails.
- Database CPU/connection/slow-query behavior is recorded.
- Concurrency does not produce negative available stock or duplicated business effect.

## Evidence

Required evidence includes target commit/revision, environment profile, dataset size, traffic model, percentile latency, error summary and relevant system metrics.

A result from a materially smaller dataset/environment cannot silently replace production-scale evidence; its limitation must be recorded.
