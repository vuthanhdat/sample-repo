# Operations Documentation

Operations được chia theo decision area để tránh một mega-document:

```text
80-operations/
├── deployment/
├── environments/
├── observability/
├── backup-recovery/
├── runbooks/
└── release/
```

`deployment` chứa quy trình triển khai và rollback. `environments` chứa khác biệt cấu hình giữa môi trường. `observability` chứa cách theo dõi hệ thống. `backup-recovery` chứa backup/restore/DR. `runbooks` chứa hướng dẫn vận hành. `release` chứa quy trình và readiness của release.

Infrastructure topology thuộc `40-design/10-common/infrastructure/`; Operations mô tả cách topology đó được vận hành.