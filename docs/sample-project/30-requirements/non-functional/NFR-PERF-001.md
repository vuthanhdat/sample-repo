---
id: NFR-PERF-001
type: NonFunctionalRequirement
category: Performance
status: Draft
version: 1
---
# NFR-PERF-001 — Performance & Scalability

- Define workload profile: `<concurrent users / RPS / data volume>`.
- Define latency target by operation class, e.g. `<P95 target>`.
- Define pagination/maximum page size for large collections.
- Define background-processing throughput if jobs exist.

Applies to: `FEATURE-001`, `FEATURE-002`, relevant APIs/screens/jobs/databases.