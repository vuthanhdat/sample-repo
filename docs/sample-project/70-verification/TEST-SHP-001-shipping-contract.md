---
id: TEST-SHP-001
type: VerificationDefinition
subtype: IntegrationContract
status: Baseline
version: 1
title: Shipping provider integration contract verification
relations:
  verifies:
    - IREQ-001
    - INT-DES-001
    - INT-SHP-001
---

# TEST-SHP-001 — Shipping Integration Contract Verification

## Required scenarios

1. Successful shipment creation returns/stores provider shipment mapping.
2. Retry same fulfillment intent does not create duplicate shipment.
3. Provider validation rejection maps to canonical error.
4. Timeout/temporary unavailable follows bounded retry policy.
5. Rate-limit response is handled separately from permanent rejection.
6. Invalid/unexpected provider response does not leak provider payload as core contract.
7. Webhook/status callback is authenticated/deduplicated when applicable.

## Test boundary

Run against provider sandbox where reliable; use contract fixture/stub for deterministic failure scenarios that sandbox cannot produce. Record which source produced each result.

## Evidence

Provider API/version, adapter revision, canonical contract version, test environment and response-category summary are attached to the verification run.
