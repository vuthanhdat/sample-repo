---
id: IREQ-001
type: IntegrationRequirement
status: Baseline
version: 1
title: Shipping provider integration requirements
relations:
  participatesIn:
    - BF-001
  satisfiedBy:
    - INT-DES-001
---

# IREQ-001 — Shipping Provider Integration

## Purpose

Khi Order đạt trạng thái `ReadyForFulfillment`, hệ thống phải có khả năng gửi yêu cầu tạo shipment sang external shipping provider và nhận/truy vấn trạng thái shipment mà không làm provider trở thành owner của Order state nội bộ.

## Contract requirements

- Outbound request phải có stable request/correlation ID.
- Duplicate/retry không được tạo nhiều shipment cho cùng fulfillment intent.
- Timeout và retry policy phải được định nghĩa; không retry vô hạn.
- Provider error phải map sang canonical internal error category.
- Raw provider payload không được leak trực tiếp vào core domain/API contract.

## Availability behavior

Nếu provider tạm thời không khả dụng, Order vẫn giữ internal state hợp lệ và shipment request ở trạng thái retryable/failed có thể quan sát được. Không rollback Order creation chỉ vì shipping provider đang down.

## Data mapping

Tối thiểu map:

- Internal Order/Fulfillment ID ↔ Provider Shipment ID.
- Recipient name/address/phone theo data protection rule.
- Package/line summary cần cho shipment.
- Provider status ↔ canonical shipment status.

## Verification

Integration contract test phải cover success, timeout, retry, duplicate request, invalid response và provider business error.
