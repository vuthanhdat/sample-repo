---
id: OBS-001
type: OperationsDesign
subtype: Observability
status: Baseline
version: 1
title: Observability design
relations:
  satisfies:
    - NFR-OPS-001
  appliesTo:
    - API-ORD-001
    - API-INV-001
    - JOB-INV-001
    - INT-SHP-001
---

# OBS-001 — Observability Design

## Correlation model

Mọi inbound request có `correlationId`; internal command/event/integration call propagate correlation và dùng `causationId` khi cần biểu diễn chuỗi nguyên nhân.

Business object identifiers như Order ID/Reservation ID được log ở structured fields khi phù hợp, không nhúng ngẫu nhiên trong free-text message.

## Core dashboards

### Order dashboard

- request rate/error/P50/P95/P99;
- create/approve/cancel outcome;
- Order state distribution;
- concurrency conflict rate.

### Inventory dashboard

- reservation success/failure/conflict;
- active/expired reservations;
- expiry job lag/failure;
- stock invariant violation counter phải luôn bằng 0.

### Integration dashboard

- shipping provider request rate/latency/error;
- rate limiting/retries;
- oldest pending integration request.

## Alert examples

- API error-rate/SLO burn vượt threshold.
- `JOB-INV-001` không có successful run trong expected window.
- Oldest expired active reservation > 10 minutes.
- Shipping backlog age vượt operational threshold.
- Database saturation/connection exhaustion ảnh hưởng traffic.

## Diagnostic rule

Một operator từ alert phải lần được tới dashboard → correlation/business ID → structured logs/traces → runbook action mà không cần ad-hoc production DB modification.

## Data protection

Sensitive customer/shipping fields bị mask/redact. Log retention/access policy khác business data retention và audit retention.
