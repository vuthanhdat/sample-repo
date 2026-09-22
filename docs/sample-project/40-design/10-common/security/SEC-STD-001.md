---
id: SEC-STD-001
type: CommonStandard
scope: Security
status: Draft
version: 1
---
# SEC-STD-001 — Authorization, Audit & Secret Handling

- Server-side permission/resource enforcement is authoritative; UI hiding is not security.
- Sensitive state-changing actions produce audit records when required by `SREQ-001`.
- Secrets never live in documentation/export/config files intended for normal project sharing.
- Logs/audit avoid leaking sensitive payloads.
- Role is mapping/policy input; feature business rules remain separate from authorization.
- Security exceptions require explicit risk/decision record.