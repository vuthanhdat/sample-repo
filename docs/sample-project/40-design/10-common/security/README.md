# Security Design Area

Security Design nên được breakdown theo concern thay vì gom authentication/authorization/privacy/protection vào một document lớn.

```text
security/
├── authentication/
├── authorization/
├── data-protection/
├── secrets-keys/
├── session-token/
└── privacy/
```

- `authentication`: identity verification, login/federation/MFA decisions.
- `authorization`: role/permission/policy model and enforcement boundaries.
- `data-protection`: encryption, masking, sensitive-data handling.
- `secrets-keys`: secret/key ownership, storage and rotation principles.
- `session-token`: token/session lifecycle and trust boundaries.
- `privacy`: privacy-specific design derived from privacy requirements.

`AUTH-DES-001.md` và `SEC-STD-001.md` vẫn giữ stable ID; project lớn có thể tách/move chúng vào sub-area phù hợp.