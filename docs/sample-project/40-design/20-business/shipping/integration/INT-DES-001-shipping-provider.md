---
id: INT-DES-001
type: DesignSpecification
subtype: ExternalInterface
status: Baseline
version: 1
title: Shipping provider integration design
relations:
  satisfies:
    - IREQ-001
  specifies:
    - INT-SHP-001
---

# INT-DES-001 — Shipping Provider Integration

## Boundary

Core Order domain gọi canonical `ShippingPort`. Provider-specific HTTP/auth/status mapping nằm trong adapter; domain/application không chứa provider DTO hoặc error code trực tiếp.

```text
Order Application
      ↓ ShippingPort
Shipping Adapter
      ↓ provider API
External Shipping Provider
```

## Outbound create shipment

Canonical request:

- `fulfillmentRequestId` — idempotency/business key.
- `orderId`.
- recipient snapshot cần thiết.
- package/line summary.
- service level.

Adapter map sang provider request và lưu mapping `fulfillmentRequestId ↔ providerShipmentId`.

## Authentication

Provider credential lấy từ secret store/configuration provider; không lưu raw secret trong project document/source. Credential rotation không yêu cầu code change.

## Timeout / retry

- Connection/request timeout bounded.
- Retry chỉ transient categories, exponential backoff + jitter.
- Create shipment retry phải dùng provider idempotency capability hoặc internal dedup/mapping; không blind retry endpoint có side effect.

## Error mapping

Provider-specific errors map sang:

```text
InvalidRequest
RejectedByProvider
RateLimited
TemporaryUnavailable
AuthenticationFailed
UnknownProviderError
```

Raw provider response có thể lưu restricted diagnostic reference nhưng không trở thành public API contract.

## Status sync

Webhook nếu provider hỗ trợ; fallback polling job chỉ khi cần. Webhook phải authenticate/signature-validate và deduplicate.

## Observability

Metrics theo provider: success rate, latency, timeout, rate-limit, retry, oldest pending request. Log có correlation/order/request IDs nhưng mask sensitive recipient data.
