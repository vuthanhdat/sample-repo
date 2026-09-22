---
id: FE-ARCH-001
type: CommonDesign
scope: Common
status: Baseline
version: 1
title: Frontend Architecture and Design Rules
appliesTo:
  - Frontend
---

# FE-ARCH-001 — Frontend Architecture and Design Rules

## Structure

Frontend được tổ chức theo feature/module nhưng dùng chung shell, routing, design system, data access và cross-cutting utilities.

```text
App Shell
 ├── Routing
 ├── Authentication / Session
 ├── Layout / Navigation
 ├── Feature Modules
 │    ├── Page
 │    ├── Components
 │    ├── Form / View Model
 │    └── Data Access
 └── Shared UI / Utilities
```

## UI state

- Server state được lấy qua data-access/query layer chung.
- Local UI state chỉ giữ state trình bày/ngắn hạn.
- Không duplicate canonical business state trong nhiều store nếu không có lý do rõ ràng.

## Form and validation

- Client-side validation giúp UX nhưng server vẫn là authority.
- Validation message phải map được tới field hoặc business error có cấu trúc.

## Navigation and authorization

Frontend có thể ẩn/disable action theo permission để cải thiện UX nhưng không được coi UI check là security boundary; backend vẫn enforce authorization.

## List/table behavior

Các screen dạng list phải dùng convention chung về pagination, sort, filter, loading, empty state, error state và refresh. Chi tiết pagination contract nằm ở `API-STD-001`.

## Localization / terminology

Text hiển thị phải đi qua resource/i18n layer; không hard-code trộn ngôn ngữ trong business screen. Business term nên có semantic key ổn định để hỗ trợ help/tooltip/automation test.

## Accessibility

Keyboard navigation, focus, label, semantic control và contrast phải tuân baseline của `NFR-UX-001`.