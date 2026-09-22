---
id: ROADMAP-001
type: Roadmap
status: Baseline
version: 1
title: Order and Inventory delivery roadmap
objects:
  - MS-001
  - MS-002
  - MS-003
---

# ROADMAP-001 — Delivery Plan

Roadmap được định nghĩa theo **outcome/deliverable state**, không theo số task hoàn thành.

## Phase 1 — Core transaction foundation

### MS-001 — Order & Inventory Data Foundation

Required outcomes:

- `DB-ORD-001` → Implemented/Verified theo migration/integrity scope.
- `DB-INV-001` → Implemented/Verified.
- Core domain/application baseline cho Order/Inventory.

Dependencies: `DBD-001`, `DBD-002` baseline.

## Phase 2 — Order flow

### MS-002 — Order Processing Flow

Required outcomes:

- `API-ORD-001` → Verified.
- `API-INV-001` → Verified.
- `EVT-ORD-001` → Verified.
- `SCR-ORD-001` → Verified.
- `JOB-INV-001` → Verified.

Dependencies: MS-001 plus relevant API/screen/job/event specs.

## Phase 3 — Integration & production readiness

### MS-003 — External Integration and Operational Readiness

Required outcomes:

- `INT-SHP-001` → Verified.
- Performance/reliability/security tests pass required release gates.
- Deployment/observability/runbook baseline complete.

## Planning rules

1. Task decomposition có thể thay đổi mà milestone outcome không đổi.
2. Contract baseline cho phép FE/BE/integration tasks chạy song song khi dependency thực tế cho phép.
3. Critical NFR remediation được lập task explicit; không giấu trong generic “polish” task.
4. Milestone không hoàn thành chỉ vì tất cả task đóng nếu required deliverable chưa Verified.
