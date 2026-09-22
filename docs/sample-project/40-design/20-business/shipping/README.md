# Shipping Design

Owner scope: Shipping Integration.

```text
shipping/
├── README.md
└── integration/
    └── INT-DES-001-shipping-provider.md
```

Shipping-specific protocol, provider mapping, retry/error mapping và canonical contract nằm ở đây. Các technical conventions chung như authentication plumbing, error handling baseline, logging/tracing và transaction policy được kế thừa từ `10-common`.