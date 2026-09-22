# Design Structure

Design được tách tuyệt đối theo scope:

```text
40-design/
├── 10-common/      # technical baseline toàn application / nhiều feature
└── 20-feature/     # design riêng từng FEATURE-*
```

Phân loại một design bằng câu hỏi: **nếu thay toàn bộ nghiệp vụ bằng một feature khác, design này còn đúng không?** Nếu còn đúng thì thuộc `10-common`; nếu không thì thuộc `20-feature/<feature-id>/`.

Trong feature branch, level tiếp theo là artifact type (`decision`, `api`, `screen`, `job`, `event`, `integration`, `data`...).

Folder là navigation projection; metadata `scope`, `feature`, `type`, ID và relations mới là model thật.