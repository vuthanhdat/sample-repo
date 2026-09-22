---
id: INT-SHP-001
type: Deliverable
subtype: ExternalInterface
status: Specified
version: 1
title: Shipping provider interface
relations:
  specifiedBy:
    - INT-DES-001
  implementsRequirements:
    - IREQ-001
---

# INT-SHP-001 — Shipping Provider Interface

## Purpose

Canonical integration deliverable giữa Order/Fulfillment scope và external shipping provider.

## Includes

- Shipping port/interface.
- Provider adapter.
- Request/response mapping.
- Idempotency/provider shipment mapping.
- Authentication/secret integration.
- Webhook/polling processing khi applicable.
- Contract/integration tests.

Provider-specific implementation artifact có thể thay đổi mà identity của `INT-SHP-001` vẫn giữ ổn định nếu canonical business contract không đổi.
