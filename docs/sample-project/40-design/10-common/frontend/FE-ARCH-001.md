---
id: FE-ARCH-001
type: CommonDesign
scope: Frontend
status: Draft
version: 1
---
# FE-ARCH-001 — Frontend Architecture

Define app shell, routing, feature/module boundary, server-state vs client-state, form handling, query/cache strategy, error/loading/empty states, localization hook and testability.

Feature UI chỉ chứa feature-specific composition/behavior; common components phải có stable contract và không nhét business logic của một feature vào shared UI.