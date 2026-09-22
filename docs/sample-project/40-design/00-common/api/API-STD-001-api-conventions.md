---
id: API-STD-001
type: CommonDesign
scope: Common
status: Baseline
version: 1
title: API Conventions
appliesTo:
  - DeliverableType:API
---

# API-STD-001 — API Conventions

## Resource and endpoint conventions

API dùng resource-oriented naming khi phù hợp. Command mang business meaning rõ ràng có thể dùng action endpoint riêng thay vì ép mọi behavior thành CRUD.

## Pagination

List endpoint mặc định phải phân trang. Contract chuẩn:

- `page`: bắt đầu từ 1.
- `pageSize`: có default và max limit.
- `sort`: field + direction theo allow-list.
- filter phải explicit, không nhận raw SQL/expression.
- response trả `items`, `page`, `pageSize`, `totalItems`, `totalPages` khi count khả thi.

Dataset lớn hoặc stream-like có thể dùng cursor pagination nếu design document ghi rõ exception.

## Error model

Error response tối thiểu có:

- stable error code;
- human-readable message;
- correlation/trace id;
- field errors nếu là validation;
- không leak stack trace hoặc secret.

## Idempotency

Command có nguy cơ retry/double-submit phải khai báo idempotency policy. Nếu dùng key, server phải định nghĩa scope, retention và behavior khi payload khác nhau dùng cùng key.

## Concurrency

API update state phải tuân `TX-STD-001`; nếu dùng optimistic concurrency thì expose version/ETag hoặc domain revision phù hợp.

## Compatibility

Breaking change phải tạo contract version hoặc approved migration plan; không âm thầm đổi field meaning/type của baseline contract.