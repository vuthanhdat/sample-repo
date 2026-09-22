---
id: API-STD-001
type: CommonStandard
scope: API
status: Draft
version: 1
---
# API-STD-001 — API Conventions

## Contract conventions
- Stable resource/action naming.
- Explicit request/response schemas; no anonymous unversioned payload conventions.
- Standard error envelope with machine code + correlation ID.
- Authorization enforced server-side.

## Collection API
Define default paging fields such as `page`, `pageSize`, `sort`, `filter` and response metadata. Cursor pagination is allowed only as documented exception for a concrete API.

## Mutations
Define idempotency where duplicate submission is plausible; use optimistic concurrency/version tokens when stale writes matter.