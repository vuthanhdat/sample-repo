# Cross-Cutting Design Area

Cross-cutting design chứa các concern áp dụng xuyên nhiều layer/feature và không nên bị lặp lại trong từng feature design.

Typical decision areas:

```text
cross-cutting/
├── error-handling/
├── observability-logging/
├── configuration/
├── resilience/
├── caching/
├── localization/
├── auditing/
└── feature-flags/
```

Không phải project nào cũng cần tất cả. Chỉ materialize area applicable.

Các file hiện có `ERR-STD-001.md` và `OBS-STD-001.md` là sample baseline cho error và observability. Khi số lượng decision tăng, tách chúng vào sub-area tương ứng thay vì để `cross-cutting/` trở thành folder phẳng rất lớn.