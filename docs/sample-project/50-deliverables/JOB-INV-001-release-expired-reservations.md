---
id: JOB-INV-001
type: Deliverable
subtype: BatchJob
status: Specified
version: 1
title: Release expired stock reservations
relations:
  specifiedBy:
    - JOB-DES-001
  implementsRequirements:
    - REQ-INV-001
  constrainedBy:
    - NFR-REL-001
    - NFR-PERF-001
---

# JOB-INV-001 — Release Expired Reservations Job

## Purpose

Background deliverable định kỳ tìm và expire active stock reservations đã quá thời gian giữ hàng.

## Contract summary

- Schedule baseline: every minute.
- Selection: Active + ExpiresAt <= now.
- Processing: paged/bounded.
- Idempotent and safe on rerun.
- Concurrency-safe against consume/cancel/release flows.
- Emits run metrics and item failure evidence.

Chi tiết kỹ thuật canonical nằm ở `JOB-DES-001`.
