---
id: AUTH-DES-001
type: CommonDesign
scope: Security
status: Draft
version: 1
---
# AUTH-DES-001 — Authentication & Session

Covers login, logout, credential/session/token lifecycle, renewal/expiry, authentication context, logout invalidation expectations and machine-vs-human identity boundary.

Authentication answers **who the principal is**. Feature-specific authorization answers **what that principal may do** and references this baseline rather than redefining login/session behavior.