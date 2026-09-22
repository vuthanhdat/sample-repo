---
id: SREQ-001
type: SecurityRequirement
status: Baseline
version: 1
title: Authentication, authorization and audit requirements
relations:
  constrains:
    - API-ORD-001
    - API-INV-001
    - SCR-ORD-001
  satisfiedBy:
    - SEC-DES-001
---

# SREQ-001 — Authentication, Authorization & Audit

## Authentication

Mọi interactive/API caller phải có authenticated identity ngoại trừ endpoint được khai báo public rõ ràng. Internal service-to-service call phải dùng service identity, không chia sẻ user credential.

## Authorization

- Order read/write action phải kiểm tra permission ở backend.
- Approval action yêu cầu role/permission phù hợp và phải kiểm tra business constraint của `BR-ORD-001`.
- Inventory adjustment/reservation administrative action phải giới hạn theo role.
- UI visibility không thay thế server-side authorization.

## Audit

Các action sau phải lưu audit event có actor, timestamp, target ID, action, result và correlation/causation metadata khi có:

- Order approve/reject/cancel.
- Manual inventory adjustment.
- Reservation administrative release/override.
- Permission-sensitive configuration change.

Audit record không được cho application user sửa/xóa qua normal business API.

## Data protection

- Sensitive customer/shipping fields không được ghi raw vào application log nếu không cần thiết.
- Secrets/API credentials không được lưu trong source/document sample.
- Transport phải dùng TLS ở production boundary.

## Verification

- `SEC-TEST-001`: authorization matrix + negative tests.
- Audit coverage phải có automated assertions cho các privileged business actions.
