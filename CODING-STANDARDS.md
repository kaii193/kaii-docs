```
 ██████╗  ██████╗     ███████╗██╗███╗   ██╗██████╗ 
██╔════╝ ██╔═══██╗    ██╔════╝██║████╗  ██║██╔══██╗
██║  ███╗██║   ██║    █████╗  ██║██╔██╗ ██║██║  ██║
██║   ██║██║   ██║    ██╔══╝  ██║██║╚██╗██║██║  ██║
╚██████╔╝╚██████╔╝    ██║     ██║██║ ╚████║██████╔╝
 ╚═════╝  ╚═════╝     ╚═╝     ╚═╝╚═╝  ╚═══╝╚═════╝ 
```

<div align="center">

### 📐 Go Find Method — Coding Standards (Baseline)

**Chuẩn code nền tảng, không phụ thuộc ngôn ngữ**

</div>

---

> [!NOTE]
> Đây là **sàn chất lượng chung** mà mọi lần implement và review đều nên đạt — không
> phải playbook cho một framework cụ thể. Hướng dẫn theo framework/pattern (frontend,
> backend, API) nằm ở các skill riêng và được **xếp chồng lên trên** baseline này.
> Nguồn gốc: skill `gf-coding-standards`.

**Cách dùng:**
- Khi **implement**: coi mỗi quy tắc bên dưới là một guardrail trong lúc viết code.
- Khi **review**: coi mỗi quy tắc là một mục trong checklist.
- Quy tắc riêng của dự án có thể **override hoặc mở rộng** baseline này — xem mục
  [Chuẩn riêng theo dự án](#-chuẩn-riêng-theo-dự-án) ở cuối.

---

## 🎯 Nguyên tắc cốt lõi

- **Readability first** — code được đọc nhiều hơn được viết. Ưu tiên tên rõ nghĩa và
  cấu trúc tự giải thích, thay vì sự "khôn ngoan" hay comment.
- **KISS** — giải pháp đơn giản nhất mà chạy được. Không tối ưu sớm, không trừu tượng
  hóa theo phỏng đoán.
- **DRY** — gom logic lặp lại vào function/module có tên; tránh copy-paste. (Nhưng
  đừng trừu tượng hóa hai thứ chỉ *tình cờ* giống nhau.)
- **YAGNI** — chỉ xây thứ yêu cầu hiện tại cần. Thêm độ phức tạp khi nó thực sự cần,
  không phải trước đó.

---

## 🏷️ Naming (Đặt tên)

- Dùng tên **mô tả rõ intent**: `marketSearchQuery`, `isUserAuthenticated`,
  `totalRevenue` — không phải `q`, `flag`, `x`.
- Function đọc theo dạng **verb + noun**: `fetchMarketData`, `calculateSimilarity`,
  `isValidEmail` — không phải `market`, `similarity`, `email`.
- Boolean đọc như một predicate: `is…`, `has…`, `should…`.
- Thay **magic number/string** bằng hằng số có tên: `const MAX_RETRIES = 3` thay vì
  số `3` trơ trọi.

---

## 🧊 Immutability (Bất biến)

- Ưu tiên cập nhật bất biến; không mutate input dùng chung tại chỗ. Tạo giá trị mới
  (copy-and-change) thay vì mutate object/array mà caller có thể còn đang giữ.
- Khi mutate tại chỗ là lựa chọn hiệu năng có chủ đích, hãy cô lập cục bộ và **ghi
  comment giải thích lý do**.

---

## 🚨 Error handling (Xử lý lỗi)

- Xử lý cả nhánh thất bại, không chỉ happy path. Validate input bên ngoài và kiểm tra
  kết quả thao tác trước khi dùng.
- Fail với thông báo rõ ràng, có ngữ cảnh; đừng nuốt lỗi âm thầm. Giữ nguyên nguyên
  nhân gốc khi re-raise.
- Đừng catch thứ bạn không xử lý được — để nó propagate lên tầng có thể xử lý.

---

## ⚡ Concurrency (Đồng thời)

- Chạy các tác vụ async độc lập **song song** thay vì tuần tự khi giữa chúng không có
  phụ thuộc dữ liệu.
- Giữ đúng thứ tự cho các việc phụ thuộc nhau; không song song hóa các thao tác phải
  quan sát kết quả của nhau.

---

## 🔒 Type safety & contracts

- Cho function và data structure kiểu/contract **rõ ràng và chính xác**; tránh escape
  hatch không kiểu (`any` và tương đương), trừ ở ranh giới thực sự — và ở đó phải
  ràng buộc lại.
- Validate dữ liệu đi qua **trust boundary** (user input, network, storage) theo một
  schema trước khi tin vào cấu trúc của nó.

---

## 💬 Comments

- Giải thích **why**, không phải **what** — code đã nói *what* rồi. Tốt: "exponential
  backoff để tránh làm quá tải API lúc outage." Tệ: "tăng counter lên 1."
- Document API public/exported: mục đích, tham số, giá trị trả về, lỗi, và một ví dụ
  ngắn khi hữu ích.
- Xóa code bị comment-out; version control đã nhớ hộ rồi.

---

## 👃 Code smells cần tránh

- **Long functions** — nếu một function làm quá nhiều việc, tách thành các bước có tên.
- **Deep nesting** — ưu tiên early return / guard clause thay vì 4–5 tầng `if`.
- **Magic numbers/strings** — đặt tên cho chúng.
- **Duplicated logic** — extract ra (cân bằng YAGNI/DRY).
- **Vague names** — đổi tên để nói rõ intent.

---

## 🧪 Testing

- Cấu trúc test theo **Arrange / Act / Assert**.
- Đặt tên test theo **hành vi + điều kiện**: "returns empty list when no markets match
  query", không phải "works".
- Test hành vi thật và các edge/failure case có ý nghĩa, không chỉ happy path.

---

## 📁 Chuẩn riêng theo dự án

Quy tắc baseline có thể bị override hoặc mở rộng theo từng dự án. Khi có, hãy tham khảo:

- `{project_knowledge}` — tài liệu và quy ước của dự án.
- `{project-root}/**/project-context.md` — quy tắc implement dành cho LLM của dự án này.
- Mọi config của linter/formatter/type-checker trong repo — **config của tool là nguồn
  chân lý** cho formatting và lint rule; theo nó thay vì bất cứ điều gì ở đây.

> [!IMPORTANT]
> Khi quy tắc riêng của dự án **xung đột** với baseline này, **quy tắc dự án thắng**.
> Các quy ước theo framework/pattern được cung cấp bởi các skill chuyên biệt, xếp
> chồng lên trên baseline này.
