# Batch / Job List

> Derived/inventory view. Batch và background job được quản lý riêng với screen/API vì có lifecycle runtime và operational concerns khác.

| ID | Name | Type | Trigger/Schedule | Module | Design Spec | Implement Task | Verification | Status |
|---|---|---|---|---|---|---|---|---|
| `JOB-INV-001` | Release Expired Reservations | Background Job | Scheduled | Inventory | `JOB-DES-001` | `TASK-JOB-001` | `REL-TEST-001` | Specified |

## Required columns

ID, name, batch/job type, trigger/schedule, owner module, input/selection, output, retry/rerun policy, design spec, task, verification, monitoring/alert và status.