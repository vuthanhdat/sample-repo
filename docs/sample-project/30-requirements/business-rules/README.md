# Business Rules

Business rule là policy/invariant chi phối behavior nhưng có thể được reuse bởi nhiều requirement, flow hoặc feature.

Tách rule thành `BR-*` riêng khi:

- rule được nhiều requirement tham chiếu;
- rule có lifecycle/version độc lập;
- rule cần trace tới compliance/policy/source riêng;
- thay đổi rule có blast radius vượt một feature.

Rule local, đơn giản và chỉ dùng trong một requirement có thể tiếp tục nằm trong Functional Requirement để tránh over-fragmentation.

Recommended contents: statement, rationale/source, applicability/scope, examples/edge cases, related requirements/features and exceptions.