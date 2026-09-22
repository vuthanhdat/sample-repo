---
id: NFR-OPS-001
type: NonFunctionalRequirement
category: ObservabilityOperability
status: Baseline
version: 1
title: Observability and operability requirements
relations:
  constrains:
    - API-ORD-001
    - API-INV-001
    - JOB-INV-001
    - INT-SHP-001
---

# NFR-OPS-001 — Observability & Operability

## Objective

Production support phải có khả năng phát hiện, chẩn đoán và khôi phục các failure chính mà không cần đọc trực tiếp database hoặc đoán trạng thái từ log rời rạc.

## Logging requirements

- Structured logging với timestamp, severity, service/module, correlation ID và relevant business object ID.
- Command quan trọng ghi result category nhưng không duplicate sensitive payload.
- Job run có JobRun ID; integration call có external request/correlation ID.
- Không dùng application log thay thế business audit/history.

## Metrics requirements

Tối thiểu phải có:

- request count/error/latency theo endpoint/result category;
- Order command success/failure;
- stock reservation success/conflict/failure;
- active/expired reservation count;
- background job success/failure/lag;
- shipping integration latency/error/rate-limit/retry;
- database connection/slow query indicators ở platform level.

## Tracing/correlation

Một business flow phải trace được tối thiểu qua Order API → Inventory/API hoặc async event/integration bằng correlation/causation metadata. Không bắt buộc distributed tracing vendor cụ thể.

## Health/readiness

- Liveness chỉ phản ánh process sống.
- Readiness phản ánh application có thể nhận traffic an toàn.
- External non-critical dependency không được làm liveness fail; readiness behavior phải theo degradation policy.

## Alerting

Alert phải dựa trên actionable symptom/SLO hoặc backlog nguy hiểm, ví dụ error rate tăng, oldest expired reservation quá ngưỡng, job liên tục fail, shipping request backlog tăng. Không alert theo mọi log error đơn lẻ.

## Verification

Operational readiness review phải chứng minh dashboard/metric/log/correlation và ít nhất một simulated failure có thể được chẩn đoán theo runbook.
