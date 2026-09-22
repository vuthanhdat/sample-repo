# Infrastructure Design Area

Infrastructure Design mô tả physical/runtime topology cần để solution chạy; khác với operational procedure trong `80-operations/`.

Recommended decision areas:

```text
infrastructure/
├── deployment-topology/   # mapping service/component to runtime/deployment units
├── compute-runtime/       # VM/container/serverless/runtime choices and sizing principles
├── network/               # network zones, ingress/egress, connectivity, DNS/TLS boundaries
├── storage/               # persistent volumes, object/file storage, durability choices
└── environment-topology/  # dev/test/staging/prod topology and isolation principles
```

Infrastructure Design trả lời HOW solution được bố trí. Deployment steps, backup procedure, incident runbook và release procedure thuộc Operations.