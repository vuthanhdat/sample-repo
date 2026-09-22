# AI-Assisted Software Engineering Principles

Dự án lớn và phức tạp làm lộ rõ một giới hạn quan trọng của AI coding: **AI rất giỏi tạo ra phương án có vẻ hợp lý, nhưng không đáng tin nếu chỉ dựa vào prompt, AGENTS.md hay tài liệu Markdown để duy trì chất lượng và tính nhất quán của cả codebase**.

Repository này ghi lại các nguyên tắc thực dụng khi dùng AI để phát triển phần mềm ở quy mô lớn.

## 1. Requirement phải rõ trước khi code

AI đặc biệt nguy hiểm khi requirement mơ hồ vì nó có xu hướng tự lấp chỗ trống bằng giả định hợp lý rồi triển khai như thể đó là requirement thật.

Cần phân biệt rõ:

```text
Business Intent
    ↓
Requirement
    ↓
Acceptance Criteria
    ↓
Design Decision
    ↓
Implementation
```

Một requirement tốt tối thiểu cần làm rõ:

- Mục tiêu nghiệp vụ.
- Actor nào tham gia.
- Input / output.
- Business rule bắt buộc.
- Boundary và ownership của dữ liệu.
- Scope / out-of-scope.
- Acceptance criteria có thể kiểm chứng.

**AI không được âm thầm biến assumption thành requirement.**

---

## 2. Documentation là knowledge, không phải enforcement

Các file như:

- `README.md`
- `AGENTS.md`
- `CLAUDE.md`
- Architecture docs
- ADR
- Design docs

vẫn rất cần thiết để giải thích:

- WHY: tại sao hệ thống được thiết kế như vậy.
- CONTEXT: bối cảnh và constraint.
- DECISION: quyết định kiến trúc nào đã được chọn.
- TRACEABILITY: requirement nào liên quan tới flow, module, API, entity, test nào.

Nhưng Markdown chỉ là **soft constraint**.

Ví dụ:

```text
AGENTS.md:
"Application layer không được phụ thuộc Infrastructure."
```

AI vẫn có thể vi phạm rule đó.

Do đó nguyên tắc là:

> **Prefer enforcement over instruction.**

Nếu một rule có thể được compiler, analyzer, architecture test, schema hoặc CI enforce thì không nên chỉ ghi nó trong tài liệu.

---

## 3. Document phải có cấu trúc và traceability

Nhiều document không đồng nghĩa với nhiều knowledge.

Một dự án lớn cần tránh việc cùng một khái niệm được copy và mô tả lại ở nhiều file khác nhau.

Nên có canonical definition và trace bằng ID:

```text
Business Flow
    ↓
System / Module
    ↓
Screen / API / Batch / Event
    ↓
Use Case
    ↓
Entity / Data
    ↓
Code
    ↓
Test
```

Ví dụ:

```text
BF-P2P-001
   ├─ API-PUR-003
   ├─ EVT-ACC-002
   ├─ UC-PUR-005
   └─ E2E-P2P-001
```

Document nên là **knowledge model có ownership và reference**, không phải kho Markdown phát triển tự do.

---

## 4. AI không được tự quyết định code mình viết là đạt chuẩn

Một lỗi phổ biến của AI coding là:

```text
AI viết code
    ↓
AI review code
    ↓
AI kết luận "đã tuân thủ Clean Architecture"
```

Cách này không đủ đáng tin.

> **AI không nên vừa là người viết code vừa là cơ chế duy nhất quyết định code đó có đạt chuẩn hay không.**

Phải có feedback độc lập từ compiler, test và tooling.

---

## 5. Quality Assurance phải có nhiều lớp

Không có một loại test hay một tool nào đủ để đánh giá toàn bộ chất lượng software.

### 5.1 Specification Quality

Kiểm tra chúng ta có đang xây đúng thứ cần xây hay không.

- Requirement review.
- Acceptance criteria.
- Business scenario.
- Definition of Done.

### 5.2 Structural Quality

Kiểm tra code có được tổ chức đúng cách hay không.

- Compiler.
- Static analyzer.
- SonarQube.
- Architecture tests.
- Dependency rules.
- Complexity checks.
- Duplication checks.

### 5.3 Behavioral Quality

Kiểm tra hệ thống có chạy đúng behavior hay không.

- Unit test.
- Integration test.
- Contract test.
- E2E test.

### 5.4 Test Quality

Kiểm tra chính test suite có đủ mạnh hay không.

- Coverage.
- Mutation testing.
- Review assertion quality.

---

## 6. Functional test không đủ để đảm bảo kiến trúc tốt

Một hệ thống hoàn toàn có thể:

```text
Build          ✅
Unit Test      ✅
Integration    ✅
E2E            ✅
```

nhưng vẫn có:

- God object.
- Business logic trong controller.
- Hardcode quá nhiều.
- Circular dependency.
- Module boundary bị xuyên thủng.
- Duplicate responsibility.
- Class phụ thuộc quá nhiều service.
- Infrastructure leak vào Application.

Do đó cần **architecture conformance test** bên cạnh functional test.

Ví dụ rule:

```text
Order.Application
    ❌ không được reference Accounting.Infrastructure
```

Rule này nên được encode thành test thay vì chỉ ghi trong `AGENTS.md`.

---

## 7. Tooling là một phần của kiến trúc phát triển

Quality không nên phụ thuộc vào việc developer hoặc AI nhớ chạy checklist thủ công.

Một toolchain thực dụng có thể gồm:

| Mục tiêu | Tool gợi ý |
|---|---|
| Build / compile | `dotnet build`, TypeScript compiler |
| Format / style | `dotnet format`, ESLint, Prettier |
| Static quality | SonarQube |
| Security static analysis | GitHub CodeQL |
| Architecture rules | NetArchTest / ArchUnitNET |
| Unit / Integration | xUnit / NUnit |
| Contract test | Pact / schema tests |
| E2E | Playwright |
| Mutation testing | Stryker.NET |
| Dependency update | Dependabot |
| Container / dependency vulnerabilities | Trivy |

Không nên kỳ vọng SonarQube giải quyết mọi vấn đề.

Ví dụ:

- SonarQube phát hiện complexity, duplication, smell.
- Architecture test phát hiện dependency sai.
- Playwright phát hiện business flow hỏng.
- Mutation testing phát hiện test suite yếu.
- CodeQL phát hiện security pattern nguy hiểm.

---

## 8. Mỗi rule quan trọng phải được đặt ở mức enforcement mạnh nhất có thể

Ví dụ:

```text
"Không duplicate code"
→ SonarQube

"Application không reference Infrastructure"
→ Architecture Test

"API phải đúng contract"
→ Contract Test

"Business flow P2P phải chạy"
→ Playwright

"Domain calculation phải đúng"
→ Unit Test

"Test phải đủ mạnh"
→ Mutation Testing

"Không merge code có vulnerability nghiêm trọng"
→ CodeQL / Trivy / Quality Gate
```

Markdown nên giữ phần:

```text
WHY
CONTEXT
DECISION
```

Machine nên giữ phần:

```text
ENFORCEMENT
```

---

## 9. Quality Gate phải chặn merge

Quality check chỉ thực sự có giá trị khi nó có khả năng chặn code xấu đi vào branch chính.

Một pipeline mong muốn:

```text
Code
  ↓
Build
  ↓
Lint / Formatter
  ↓
Static Analysis
  ↓
Architecture Test
  ↓
Unit Test
  ↓
Integration Test
  ↓
Contract Test
  ↓
E2E
  ↓
Mutation Test
  ↓
Quality Gate
  ↓
PASS → Merge
FAIL → Fix
```

Không nên coi:

```text
"AI đã review và nói OK"
```

là quality gate.

---

## 10. AGENTS.md nên là bản hướng dẫn sử dụng hệ thống kiểm chứng

Không nên cố biến `AGENTS.md` thành cuốn sách vài nghìn dòng với hy vọng AI sẽ nhớ và tuân thủ tuyệt đối.

Một `AGENTS.md` tốt nên trả lời ngắn gọn:

- Repository được tổ chức thế nào.
- Module ownership ở đâu.
- Những invariant quan trọng nào tồn tại.
- Những command nào bắt buộc phải chạy.
- Definition of Done là gì.
- Tool / test nào enforce từng rule.

Ví dụ:

```text
Before completing a task, run:

./build
./test
./architecture-check
./lint
./e2e
```

Tức là `AGENTS.md` nên **dẫn AI tới hệ thống kiểm chứng**, thay vì tự đóng vai hệ thống kiểm chứng.

---

## 11. Closed Feedback Loop cho AI coding

Mô hình mong muốn:

```text
Requirement
    ↓
AI Implementation
    ↓
Code
    ↓
Automated Checks
    ├─ Compiler
    ├─ SonarQube
    ├─ Architecture Tests
    ├─ CodeQL
    ├─ Unit / Integration
    ├─ Contract Test
    ├─ Playwright
    └─ Mutation Test
    ↓
Quality Gate
    ↓
PASS ─────────→ Merge
FAIL ─────────→ AI sửa
                  ↓
               chạy lại
```

Đây là **closed feedback loop**.

AI có thể mắc lỗi, nhưng môi trường phải làm cho lỗi đó được phát hiện sớm và sửa trước khi merge.

---

## 12. Nguyên tắc tổng kết

### Requirement

> Requirement phải đủ rõ để AI không phải đoán.

### Documentation

> Document phải tổ chức knowledge và giải thích WHY; không nên được coi là cơ chế enforcement chính.

### Architecture

> Kiến trúc tốt không chỉ nằm trên sơ đồ. Các boundary quan trọng phải có khả năng được kiểm tra tự động.

### Testing

> Test không chỉ để chứng minh code chạy được; test phải bảo vệ business behavior và regression.

### Tooling

> Những rule quan trọng nên được chuyển từ prose thành executable constraints càng nhiều càng tốt.

### AI

> Mục tiêu không phải làm AI không bao giờ sai. Mục tiêu là xây một môi trường mà AI làm sai thì lỗi được phát hiện nhanh, rõ và có thể sửa lặp lại.

---

## Một câu để nhớ

> **Đừng chỉ cố viết prompt để AI làm đúng. Hãy xây hệ thống khiến AI khó làm sai mà không bị phát hiện.**
