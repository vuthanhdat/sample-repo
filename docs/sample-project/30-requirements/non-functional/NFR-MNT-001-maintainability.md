---
id: NFR-MNT-001
type: NonFunctionalRequirement
category: MaintainabilityModifiability
status: Baseline
version: 1
title: Maintainability and modifiability baseline
relations:
  constrains:
    - ARCH-002
    - API-ORD-001
    - API-INV-001
    - JOB-INV-001
---

# NFR-MNT-001 — Maintainability & Modifiability

## Objective

Thay đổi một business capability không được mặc định yêu cầu sửa xuyên nhiều module không sở hữu capability đó. Kiến trúc/code phải giữ ownership và dependency đủ rõ để change impact có thể dự đoán và kiểm chứng.

## Requirements

1. Order và Inventory giữ module boundary như `ARCH-002`; không direct-reference infrastructure/data model của module khác.
2. Business rules quan trọng có canonical implementation/ownership, không copy threshold/state logic ở UI/API/job thành nhiều nguồn độc lập.
3. Public/internal contracts quan trọng có version/test và không thay đổi breaking âm thầm.
4. Component có responsibility quá rộng, dependency fan-in/out bất thường hoặc circular dependency phải bị architecture/static review phát hiện.
5. Repetitive configuration/boilerplate có thể common hóa nhưng domain semantics khác nhau không được ép chung chỉ để giảm line count.
6. Refactor không được thay đổi public behavior nếu không có linked requirement/change.

## Enforceable checks

- architecture tests cho layer/module dependency;
- static analysis/complexity/duplication gate;
- contract tests;
- unit/integration regression tests;
- code review ownership rules.

## Verification

Release gate phải chạy architecture/static checks; violation cần explicit approved exception/change, không được bỏ qua chỉ vì functional E2E vẫn pass.
