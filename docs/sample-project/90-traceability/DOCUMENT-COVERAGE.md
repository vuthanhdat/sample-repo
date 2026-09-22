# Document Coverage

Derived checklist dùng để kiểm tra completeness, không phải source of truth.

## Knowledge Coverage
| Product | Status |
|---|---|
| Goals | Present |
| Project Context / Actors / Glossary | Present |
| Business Flows | Present |
| Feature Index | Present |
| Functional Requirements | Present |
| NFR applicability review | Present |
| Data / Integration / Security requirements | Present |

## Common Design Coverage
| Product | Status |
|---|---|
| Application Architecture | Present |
| Backend Standard | Present |
| Frontend Standard | Present |
| API / Pagination Standard | Present |
| Transaction / Concurrency Standard | Present |
| Data Standard | Present |
| Authentication / Authorization | Present |
| Error / Observability Standard | Present |

## Feature Design Coverage
| Feature | Decision | API | Screen | Job | Event | Integration | Data |
|---|---|---|---|---|---|---|---|
| `FEATURE-001` | Yes | Yes | Yes | N/A | Yes | N/A | Yes |
| `FEATURE-002` | Yes | Yes | N/A | Yes | N/A | Yes | Yes |

## Output Inventory Coverage
Screen/API/Job/Interface/Event/Database lists are present. File/Mail/Notification/Report are `Not Evaluated`, intentionally not populated with fake deliverables.

A file existing does not mean coverage complete: required relations, lifecycle/version, task and verification coverage must also pass.