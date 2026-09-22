# Relation Vocabulary

Relation là structured edge trong graph, không suy luận từ folder.

| Relation | Ý nghĩa |
|---|---|
| `supports` | object hỗ trợ goal/capability |
| `participates-in` | feature/requirement tham gia flow |
| `decomposes-to` | object được phân rã thành object khác |
| `governed-by` | bị chi phối bởi rule/policy |
| `accepted-by` | có acceptance criterion |
| `constrained-by` | bị constrain bởi NFR/common standard |
| `satisfied-by` | requirement được đáp ứng bởi design |
| `introduces` | design decision tạo ra deliverable |
| `specifies` | detailed design mô tả deliverable |
| `implements` | task thực hiện deliverable/design |
| `verifies` | verification kiểm chứng target |
| `depends-on` | dependency có hướng |
| `applies-to` | common rule/design áp dụng target scope |
| `supersedes` | version/object thay thế object trước |
| `impacts` | change có potential impact |

Reverse view được derive, không lưu hai edge độc lập.