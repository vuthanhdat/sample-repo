# Non-functional Requirement Applicability Checklist

Đây là checklist theo ProjectTemplate. Mỗi category phải được đánh giá; không được bỏ qua chỉ vì không có người nhớ hỏi.

| Category | Applicability | Canonical requirement / rationale |
|---|---|---|
| Performance | Applicable | `NFR-PERF-001` |
| Scalability | Applicable | `NFR-PERF-001` |
| Availability | Applicable | `NFR-REL-001` |
| Reliability / Consistency | Applicable | `NFR-REL-001` |
| Disaster Recovery | Applicable | RPO/RTO trong `NFR-REL-001`, deployment/runbook evidence |
| Security | Applicable | `SREQ-001` |
| Privacy / sensitive data | Applicable | `SREQ-001`, `DREQ-001` |
| Observability / Operability | Applicable | `NFR-OPS-001` |
| Usability / Accessibility | Applicable | `NFR-UX-001` |
| Maintainability / Modifiability | Applicable | `NFR-MNT-001`, `ARCH-002` |
| Data retention / auditability | Applicable | `DREQ-001`, `SREQ-001` |
| External interoperability | Applicable | `IREQ-001`, `INT-DES-001` |
| Localization / i18n | Not Applicable | Sample has no multilingual requirement in current scope |
| Browser/device compatibility | Applicable | Supported browser baseline belongs to `NFR-UX-001` |
| Regulatory domain compliance | Not Applicable | No regulated-industry requirement assumed in sample; must be revisited for real project |
| Offline operation | Not Applicable | Product requires online backend connectivity |
| Multi-region active-active | Not Applicable | Explicitly out of current availability baseline |

## Rule

`Not Applicable` is a deliberate project decision with rationale, not equivalent to “nobody documented it”. When scope changes, checklist item can become Applicable and trigger requirement/design impact analysis.
