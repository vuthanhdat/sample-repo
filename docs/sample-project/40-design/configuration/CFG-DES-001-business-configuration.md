---
id: CFG-DES-001
type: DesignSpecification
subtype: Configuration
status: Baseline
version: 1
title: Business configuration design
relations:
  governs:
    - BR-ORD-001
    - JOB-INV-001
---

# CFG-DES-001 — Business Configuration Design

## Purpose

Tách các giá trị nghiệp vụ/operational có lý do thay đổi trong runtime khỏi hard-code, nhưng không biến toàn bộ behavior thành dynamic configuration khó kiểm soát.

## Configuration items

| Key | Meaning | Example | Change authority |
|---|---|---|---|
| `order.approval.highValueThreshold` | Threshold yêu cầu approval | `50000000 VND` | Operations/Admin |
| `inventory.reservation.ttlMinutes` | Thời gian giữ reservation | `30` | Operations/Admin |
| `shipping.defaultServiceLevel` | Default shipping service | `standard` | Operations/Admin |

## Rules

1. Configuration có stable key/type/schema; không lưu arbitrary JSON không validation.
2. Business-significant config change phải audit actor, before/after và effective time.
3. Secret không phải business configuration; secret nằm ở secret store.
4. Config có default hợp lệ nhưng application không âm thầm fallback nếu missing value làm thay đổi business contract nguy hiểm.
5. Nếu config change làm ảnh hưởng behavior baseline đáng kể, change management phải đánh giá requirement/test impact.

## Version/effective behavior

Approval threshold được resolve tại thời điểm policy evaluation và value/effective version cần đủ khả năng audit. Historical Order không được bị diễn giải lại theo threshold mới nếu business decision đã xảy ra trước đó.

## Verification

Test boundary values, invalid config rejection, missing config behavior, audit và runtime refresh/caching semantics nếu implementation dùng cache.
