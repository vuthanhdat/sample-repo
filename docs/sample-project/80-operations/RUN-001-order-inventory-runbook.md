---
id: RUN-001
type: Runbook
status: Baseline
version: 1
title: Order and Inventory operational runbook
relations:
  uses:
    - OBS-001
    - DEP-001
    - JOB-INV-001
    - INT-SHP-001
---

# RUN-001 — Order & Inventory Runbook

## 1. API error spike

1. Check Order/Inventory error-rate and latency dashboard.
2. Split by endpoint/result category; identify dependency/concurrency/DB pattern.
3. Use correlation IDs for representative failures.
4. Check recent deployment/configuration changes.
5. If rollback criteria met, follow deployment rollback/roll-forward policy; do not modify Order state directly in DB as first response.

## 2. Expired reservation backlog

Trigger: oldest active expired reservation exceeds alert threshold.

1. Check `JOB-INV-001` last successful run, failure count and lag.
2. Check worker health and DB contention.
3. If scheduler failed but application operation is healthy, rerun job using supported operation.
4. Verify backlog decreases and no negative/double-release invariant violation.
5. Persistent item failures are reviewed individually with Reservation ID and last error.

## 3. Shipping provider unavailable

1. Confirm provider-specific failure rate/timeout.
2. Verify internal Order state remains consistent and requests are queued/retryable.
3. Do not manually create duplicate shipments without checking fulfillment/provider mapping.
4. Pause/reduce retry if provider rate-limit/outage makes retries harmful.
5. After recovery, monitor backlog age and duplicate protection.

## 4. Database recovery

Follow approved backup/restore procedure; record restore point, target environment and validation result. After restore, run integrity checks on Order status/current state and Inventory balances/reservations before reopening writes.

## 5. Escalation evidence

Incident handoff includes time window, affected object IDs, deployment/config revision, dashboards, representative correlation IDs and mitigation already attempted.
