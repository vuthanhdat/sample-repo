# ID Conventions

Mỗi object có technical ID bất biến trong SaaS và một project-scoped human key. Sample chỉ minh họa human key.

| Object | Pattern | Example |
|---|---|---|
| Goal | `GOAL-NNN` | `GOAL-001` |
| Business Flow | `BF-NNN` | `BF-001` |
| Feature | `FEATURE-NNN` | `FEATURE-001` |
| Functional Requirement | `REQ-FNNN-NNN` | `REQ-F001-001` |
| Business Rule | `BR-FNNN-NNN` | `BR-F001-001` |
| Acceptance Criterion | `AC-FNNN-NNN-NN` | `AC-F001-001-01` |
| NFR | `NFR-<CATEGORY>-NNN` | `NFR-PERF-001` |
| Design Decision | `DES-FNNN-NNN` | `DES-F001-001` |
| API Design | `API-DES-FNNN-NNN` | `API-DES-F001-001` |
| Screen Design | `SCR-DES-FNNN-NNN` | `SCR-DES-F001-001` |
| Job Design | `JOB-DES-FNNN-NNN` | `JOB-DES-F002-001` |
| Deliverable | `<TYPE>-FNNN-NNN` | `API-F001-001` |
| Task | `TASK-FNNN-NNN` | `TASK-F001-001` |
| Verification | `TEST-FNNN-NNN` | `TEST-F001-001` |
| Change Request | `CR-NNN` | `CR-001` |

Filename là projection dễ đọc; key/technical ID mới là identity.