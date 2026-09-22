# Frontend Design Area

Frontend design có thể được breakdown theo các decision area:

```text
frontend/
├── architecture/         # frontend module/component boundaries and dependency rules
├── ui-ux/                # design system, layout, accessibility, interaction standards
├── state-data/           # local/global/server state, caching, synchronization
└── navigation-routing/   # route model, navigation, guards, deep-link behavior
```

Các file `FE-ARCH-001.md`, `UI-STD-001.md` hiện tại vẫn là sample baseline. Khi project lớn lên, tách các quyết định state/data flow và routing khỏi architecture tổng quát để tránh mega-document.

Screen-specific fields/actions/states thuộc Feature Design `screen/`; common frontend chỉ giữ conventions áp dụng nhiều feature.