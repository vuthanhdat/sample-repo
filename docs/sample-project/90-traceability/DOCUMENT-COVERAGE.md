# Document Coverage

Derived checklist dùng để kiểm tra completeness, không phải source of truth.

## Knowledge Coverage
| Product | Status |
|---|---|
| Goals | Present |
| Project Context | Present |
| Scope / System Boundary | Present |
| Actors / Glossary | Present |
| External Systems | Present |
| Assumptions / Constraints | Present |
| Business Flows | Present |
| Feature Index | Present |
| Functional Requirements | Present |
| Business Rules applicability | Present |
| NFR applicability review | Present |
| Data / Integration / Security requirements | Present |

## Common Design Coverage
| Product | Status |
|---|---|
| Application / System Architecture | Present |
| Module / Runtime Architecture | Defined in taxonomy; materialize as applicable |
| Backend Standard | Present |
| Frontend Standard | Present |
| API / Pagination Standard | Present |
| Integration Design | Present taxonomy |
| Transaction / Concurrency Standard | Present |
| Data Standard + lifecycle/migration taxonomy | Present |
| Authentication / Authorization | Present |
| Infrastructure / Deployment Topology | Present |
| Error / Observability / Cross-cutting Standard | Present |

## Feature Design Coverage
| Feature | Decision | Domain | Workflow/State | API | Screen | Job | Event | Integration | Data | File | Mail/Notification | Report |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| `FEATURE-001` | Yes | Not Evaluated | Not Evaluated | Yes | Yes | N/A | Yes | N/A | Yes | N/A | N/A | N/A |
| `FEATURE-002` | Yes | Not Evaluated | Not Evaluated | Yes | N/A | Yes | N/A | Yes | Yes | N/A | N/A | N/A |

## Operations Coverage
| Area | Status |
|---|---|
| Deployment Operations | Present taxonomy |
| Environment Operations | Present taxonomy |
| Observability Operations | Present |
| Backup / Recovery | Present taxonomy |
| Runbooks | Present |
| Release Operations | Present taxonomy |

## Output Inventory Coverage
Screen/API/Job/Interface/Event/Database lists are present. File/Mail/Notification/Report are `Not Evaluated`, intentionally not populated with fake deliverables.

A file existing does not mean coverage complete: required relations, lifecycle/version, task and verification coverage must also pass. `Present taxonomy` nghĩa là decision area đã được định nghĩa nhưng project sample chưa tạo fake artifact chỉ để đánh dấu hoàn thành.