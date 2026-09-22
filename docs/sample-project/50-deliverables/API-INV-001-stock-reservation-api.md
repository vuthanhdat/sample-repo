---
id: API-INV-001
type: Deliverable
subtype: API
status: Specified
version: 1
title: Stock Reservation API
relations:
  introducedBy:
    - DES-INV-001
  implementsRequirements:
    - REQ-INV-001
  implementedBy:
    - TASK-INV-BE-001
  verifiedBy:
    - TEST-INV-001
---

# API-INV-001 — Stock Reservation API

## Purpose

Cung cấp contract để reserve và release stock theo Order.

## Planned operations

- `POST /stock-reservations`
- `POST /stock-reservations/{reservationId}/release`
- `GET /stock-reservations/{reservationId}`

## Contract responsibilities

- Kiểm tra available stock.
- Reserve stock theo nguyên tắc all-or-nothing.
- Hỗ trợ idempotency.
- Release reservation an toàn khi retry.
- Không cho available stock âm.

## Traceability

`DES-INV-001` → `API-INV-001` → `TASK-INV-BE-001` → `TEST-INV-001`.
