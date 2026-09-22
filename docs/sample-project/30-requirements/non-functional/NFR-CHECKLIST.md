# NFR Applicability Checklist

Mỗi category phải được đánh giá `Applicable`, `Not Applicable + reason` hoặc `Not Evaluated`. Checklist này giúp tránh bỏ quên quality attributes; không có nghĩa mọi project phải tạo requirement cho mọi category.

| Category | Status | Requirement |
|---|---|---|
| Performance / Scalability / Capacity | Applicable | `NFR-PERF-001` |
| Availability / Reliability / Recoverability / DR | Applicable | `NFR-REL-001` |
| Observability / Operability / Supportability | Applicable | `NFR-OPS-001` |
| Usability / Accessibility | Applicable | `NFR-UX-001` |
| Maintainability / Modifiability / Testability | Applicable | `NFR-MNT-001` |
| Security / Privacy | Applicable | `SREQ-001` |
| Compatibility / Interoperability | Not Evaluated | — |
| Portability / Deployability | Not Evaluated | — |
| Localization / Internationalization | Not Evaluated | — |
| Compliance / Auditability | Not Evaluated | — |
| Offline / Intermittent Connectivity | Not Evaluated | — |
| Safety / Fail-safe Behavior | Not Evaluated | — |

Các requirement về data quality/integrity/retention có thể nằm trong `../data/`; security/privacy-specific constraints có thể nằm trong `../security/`. Checklist chỉ đảm bảo category đã được xem xét và trace tới requirement phù hợp.