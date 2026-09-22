---
id: NFR-UX-001
type: NonFunctionalRequirement
category: UsabilityAccessibility
status: Baseline
version: 1
title: Usability and accessibility baseline
relations:
  constrains:
    - SCR-ORD-001
---

# NFR-UX-001 — Usability & Accessibility

## Usability

- Order Detail phải cho user nhận biết current state và next valid action mà không phải suy luận từ raw codes.
- Loading, empty, validation, conflict và dependency-failure states có presentation riêng.
- Destructive/irreversible action phải có confirmation phù hợp với risk và hiển thị result rõ.
- Error message cho human có thể đọc nhưng machine behavior vẫn dựa trên stable error code.

## Accessibility baseline

- Main interactive flow usable bằng keyboard.
- Action/control có accessible name/label.
- Order status và error không chỉ phân biệt bằng màu.
- Focus state rõ và focus được quản lý hợp lý sau dialog/command result.
- Semantic heading/table/control structure được ưu tiên thay vì div-only custom UI.
- Target baseline: WCAG 2.1 AA cho core business flow ở mức project sample.

## Compatibility

Support current + previous major versions of Chrome/Edge for desktop-first corporate use in sample baseline. Additional browser/mobile support must be explicit instead of assumed.

## Verification

Screen tests include keyboard/action-state checks; critical flow receives accessibility scan/manual review before release baseline.
