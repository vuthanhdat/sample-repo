# Common / Application Design

Common Design là technical baseline áp dụng cho toàn application hoặc nhiều feature. Nó **không chứa entity/rule/state của một feature cụ thể**.

Các feature design mặc định kế thừa common baseline. Chỉ override khi có exception/decision được quản lý.

```text
Common Design
   ↓ applies-to
Feature Design
   ↓ specifies
Deliverable
   ↓ implemented-by
Task
```

Common changes có blast radius lớn và phải impact-analysis theo scope.