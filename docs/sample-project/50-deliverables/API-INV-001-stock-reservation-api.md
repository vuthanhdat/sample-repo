---
id: API-INV-001
type: Deliverable
subtype: API
status: Specified
version: 2
title: Stock Reservation API
relations:
  introducedBy:
    - DES-INV-001
  specifiedBy:
    - API-DES-002
  implementsRequirements:
    - REQ-INV-001
  constrainedBy:
    - NFR-PERF-001
    - NFR-REL-001
    - SREQ-001
  implementedBy:
    - TASK-INV-BE-001
  verifiedBy:
    - TEST-INV-001
    - PERF-001
    - REL-TEST-001
    - SEC-TEST-001
---

# API-INV-001 — Stock Reservation API

## Purpose

Cung cấp canonical boundary để reserve/release/query Stock Reservation theo Order. Detailed contract nằm ở `API-DES-002`.

## Operations

- `POST /api/stock-reservations`
- `POST /api/stock-reservations/{reservationId}/release`
- `GET /api/stock-reservations/{reservationId}`

## Responsibilities

- Enforce all-or-nothing reservation policy.
- Protect non-negative available stock under concurrency.
- Support idempotent reserve/release retry.
- Persist data qua `DB-INV-001`.
- Meet linked performance/reliability/security NFRs.

## Traceability

`REQ-INV-001` → `DES-INV-001` → `API-INV-001` ← `API-DES-002` → implementation/verification tasks.
