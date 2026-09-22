# Interface List

> Derived/inventory view cho mọi giao tiếp system-to-system: API external, event, file transfer, queue/topic, SFTP, webhook...

| ID | Interface | Direction | External System | Protocol | Requirement | Design Spec | Implement Task | Status |
|---|---|---|---|---|---|---|---|---|
| `INT-SHP-001` | Shipping Provider Interface | Outbound | Shipping Provider | HTTPS/JSON | `IREQ-001` | `INT-DES-001` | `TASK-INT-001` | Specified |

## Required columns

ID, name, direction, producer/consumer, protocol, data contract, auth, retry/idempotency, SLA, requirement, design spec, task, verification và status.