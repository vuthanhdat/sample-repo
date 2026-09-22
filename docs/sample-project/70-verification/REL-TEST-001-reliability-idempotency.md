---
id: REL-TEST-001
type: VerificationDefinition
subtype: Reliability
status: Baseline
version: 1
title: Reliability, concurrency and idempotency verification
relations:
  verifies:
    - NFR-REL-001
    - API-ORD-001
    - API-INV-001
    - JOB-INV-001
    - EVT-ORD-001
---

# REL-TEST-001 — Reliability & Idempotency Verification

## Required scenarios

1. Retry Create/Approve/Cancel command with same idempotency identity does not duplicate business effect.
2. Concurrent stock reservation for the same constrained stock never makes available quantity negative.
3. Duplicate reservation release/expiry does not return stock twice.
4. Process restarts after business commit but before event publish; required event is eventually published once-or-more with idempotent identity.
5. Expiry job restarts mid-page and safely reruns the same candidates.
6. Shipping provider timeout/temporary unavailable does not corrupt Order state and request remains observable/retryable.
7. Stale optimistic-concurrency update is rejected rather than silently overwriting newer state.

## Recovery evidence

For selected scenarios record state before failure, injected failure, state after recovery, retry count and final invariant check.

## Pass condition

All business invariants in `NFR-REL-001` remain true under required failure/concurrency scenarios. Merely returning HTTP success after retry is not sufficient evidence.
