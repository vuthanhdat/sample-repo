---
id: SEC-DES-001
type: DesignSpecification
subtype: Security
status: Baseline
version: 1
title: Authorization and audit design
relations:
  satisfies:
    - SREQ-001
  appliesTo:
    - API-ORD-001
    - API-INV-001
    - SCR-ORD-001
---

# SEC-DES-001 — Authorization & Audit Design

## Authorization model

Backend permission checks use action/resource semantics rather than UI role-name checks. Example permissions:

```text
order.read
order.create
order.approve
order.cancel
inventory.read
inventory.reserve
inventory.adjust
```

Role is a mapping to permissions; business rule still applies after permission check. Having `order.approve` does not bypass approval threshold, state or separation-of-duty rule.

## Enforcement points

1. API endpoint authenticates caller and resolves principal.
2. Application authorization policy verifies permission/resource scope.
3. Domain/application business rule validates state/actor constraints.
4. Audit event records sensitive successful/failed action according to policy.

UI permission only controls affordance and cannot be the sole enforcement point.

## Audit event schema

- audit event ID;
- occurred at;
- principal/actor ID and type;
- action;
- target type + ID;
- result: success/denied/failed;
- reason/error category;
- correlation ID;
- selected before/after metadata for business-significant state transitions.

Audit store is append-oriented and normal business APIs cannot mutate historical audit records.

## Sensitive data

Shipping/customer data is redacted from logs/audit unless a specific field is required for audit purpose. Diagnostic payload access is stricter than ordinary Order read permission.

## Verification

Authorization tests must include positive and negative matrix, horizontal access attempt, stale UI permission case and privileged audit assertions.
