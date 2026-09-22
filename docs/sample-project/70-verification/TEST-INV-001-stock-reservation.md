---
id: TEST-INV-001
type: VerificationDefinition
status: Baseline
version: 1
title: Verify stock reservation and release
relations:
  verifies:
    - REQ-INV-001
    - API-INV-001
---

# TEST-INV-001 — Stock Reservation Verification

## Purpose

Xác minh stock reservation/release tuân thủ requirement và không gây oversell hoặc double update khi retry.

## Covered objects

- `REQ-INV-001`.
- `BR-INV-001`.
- `AC-INV-001-01`.
- `AC-INV-001-02`.
- `API-INV-001`.

## Required scenarios

1. Reserve thành công khi tất cả item đủ hàng.
2. Reserve thất bại atomically khi một item thiếu hàng.
3. Available stock không bao giờ âm.
4. Retry cùng idempotency key không trừ tồn kho lần hai.
5. Release trả stock về đúng số lượng.
6. Retry release không cộng stock lần hai.

## Pass condition

Tất cả required scenario pass trên target revision hiện tại của `API-INV-001`.
