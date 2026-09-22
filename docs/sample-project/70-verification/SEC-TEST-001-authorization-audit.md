---
id: SEC-TEST-001
type: VerificationDefinition
subtype: Security
status: Baseline
version: 1
title: Authorization and audit verification
relations:
  verifies:
    - SREQ-001
    - SEC-DES-001
    - API-ORD-001
    - API-INV-001
---

# SEC-TEST-001 — Authorization & Audit Verification

## Authorization matrix

Test both allowed and denied paths for representative principals:

- Order reader.
- Order creator/operator.
- Order approver.
- Inventory operator.
- Unauthorized authenticated user.

## Required negative scenarios

1. User without `order.approve` calls approval endpoint.
2. UI hides action but caller manually invokes API without permission.
3. Authorized role attempts action on invalid Order state.
4. Caller attempts horizontal access to resource outside permitted scope when scoping applies.
5. Inventory adjustment/reservation administrative operation invoked without permission.

## Audit assertions

Privileged actions generate append-oriented audit record containing principal, target, action, result, timestamp and correlation metadata. Negative authorization attempts that policy marks auditable must also create evidence.

## Sensitive-data assertions

Logs/audit must not contain raw secrets or unnecessary full shipping/customer sensitive payloads.

## Pass condition

Permission enforcement is proven at backend boundary and business constraints still apply after permission check. UI behavior alone cannot satisfy this test.
