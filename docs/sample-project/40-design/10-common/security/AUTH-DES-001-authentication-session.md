---
id: AUTH-DES-001
type: CommonDesign
scope: Common
status: Baseline
version: 1
title: Authentication and Session Design
appliesTo:
  - Project
---

# AUTH-DES-001 — Authentication and Session Design

## Scope

Định nghĩa behavior chung cho login, logout, session/token lifecycle và authorization context. Business feature không tự thiết kế lại authentication riêng.

## Login

1. User gửi credential qua secure transport.
2. Authentication provider xác minh identity.
3. Hệ thống tạo session/token theo policy.
4. Response không expose secret hoặc credential detail.
5. Failed login được audit theo security policy nhưng không leak lý do giúp account enumeration.

## Session lifecycle

- Session/token có expiration rõ ràng.
- Refresh/renewal có policy riêng và có thể revoke.
- Permission/role change phải có cơ chế để session stale không tồn tại vô hạn.
- Authentication context được đưa vào request pipeline; business code không tự parse token.

## Logout

Logout phải invalidate/revoke session theo khả năng của authentication mechanism, xóa client session state và đưa user về unauthenticated route. Logout không chỉ là chuyển màn hình.

## Authorization

Authentication trả lời "ai đang gọi"; authorization trả lời "được phép làm gì". Authorization được enforce ở backend/application boundary. Frontend permission check chỉ phục vụ UX.

## Security baseline

- Secure cookie/token handling.
- CSRF protection khi cơ chế session yêu cầu.
- Không log access token/refresh token/password.
- Audit các security-relevant action.
- Rate limit/lockout policy được cấu hình theo security requirement.

## Business integration

Các business screen/API chỉ khai báo permission/policy cần thiết, ví dụ `Order.Approve`; không tự định nghĩa login/logout flow.