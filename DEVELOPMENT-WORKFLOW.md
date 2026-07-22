```
 ██████╗  ██████╗     ███████╗██╗███╗   ██╗██████╗ 
██╔════╝ ██╔═══██╗    ██╔════╝██║████╗  ██║██╔══██╗
██║  ███╗██║   ██║    █████╗  ██║██╔██╗ ██║██║  ██║
██║   ██║██║   ██║    █████╗  ██║██║╚██╗██║██║  ██║
╚██████╔╝╚██████╔╝    ██║     ██║██║ ╚████║██████╔╝
 ╚═════╝  ╚═════╝     ╚═╝     ╚═╝╚═╝  ╚═══╝╚═════╝ 
```

<div align="center">

### 🔧 Go Find Method — Development Workflow

**Commit convention · PR template · CI gates**

</div>

---

> [!NOTE]
> File này ghi lại quy trình bắt buộc khi làm việc với Git trong repo: cách đặt tên
> branch, viết commit message, mở PR, và những cổng (gate) CI phải vượt qua trước khi
> merge. Đi kèm với [Coding Standards](./CODING-STANDARDS.md) — chuẩn *chất lượng code*,
> còn file này là chuẩn *quy trình đưa code vào repo*.

---

## 🌿 Branch naming

Format: `<type>/<mô-tả-ngắn-tiếng-Anh-hoặc-không-dấu>`

| Prefix | Dùng khi |
|---|---|
| `feat/` | Tính năng mới |
| `fix/` | Sửa lỗi |
| `chore/` | Thay đổi không ảnh hưởng logic (build, deps) |
| `docs/` | Thêm/sửa tài liệu |
| `ref/` | Refactor (không đổi behavior) |
| `perf/` | Tối ưu performance |
| `hotfix/` | Fix gấp production — viết đầy đủ, không viết tắt |
| `test/` | Viết/sửa test |
| `style/` | Format, không đổi logic |

Ví dụ: `feat/listing-search-filter`, `fix/auth-token-expiry`

**Quy tắc đặt tên:**
- Mô tả ngắn gọn, viết thường, nối bằng dấu `-`.
- Không chứa tên người; ticket ID không đứng một mình mà không kèm mô tả (tránh
  `fix/JIRA-123` trơ trọi — nên là `fix/jira-123-auth-token-expiry`).
- Chọn đúng prefix theo **bản chất** thay đổi, không mặc định `feat/` cho mọi thứ.

---

## ✍️ Commit convention (Conventional Commits)

Format: `<type>(<scope>): <mô tả ngắn>`

| Type | Dùng khi |
|---|---|
| `feat` | Thêm tính năng mới |
| `fix` | Sửa bug |
| `chore` | Thay đổi không ảnh hưởng logic (build, deps) |
| `docs` | Cập nhật tài liệu |
| `refactor` | Refactor (không đổi behavior) |
| `test` | Thêm hoặc sửa test |
| `hotfix` | Fix gấp production |

Ví dụ: `feat(listing): add price filter to search API`

**Quy tắc:**
- `scope` là tên module/domain bị ảnh hưởng (vd `listing`, `auth`, `payment`), viết thường.
- Mô tả ngắn ở **thì hiện tại**, không viết hoa chữ đầu, không có dấu `.` ở cuối.
- Mỗi commit là **một thay đổi logic hoàn chỉnh** — không gộp nhiều việc không liên quan.
- `type` của commit phải **khớp với prefix của branch** (branch `fix/...` thì commit
  dùng `fix(...)`, không trộn `feat` vào branch `fix/`).

---

## 📦 PR size guidelines

| Kích thước | Lines changed | Xử lý |
|---|---|---|
| Small | < 200 | Ưu tiên review trong ngày |
| Medium | 200–500 | Review trong 1–2 ngày |
| Large | > 500 | **Trao đổi với FSD trước khi mở PR** — có thể cần tách nhỏ |

**Quy tắc:**
- Trước khi mở PR, ước tính số dòng thay đổi: `git diff --stat` so với branch base.
  Nếu vượt **500 dòng**, dừng lại và tách PR theo từng tính năng/module độc lập.
- PR ở mức Medium/Large: description phải liệt kê rõ các phần thay đổi theo file/module
  để reviewer dễ theo dõi.
- **Không gộp refactor lớn chung với feature mới** trong cùng 1 PR — tách PR `ref/`
  riêng, merge xong rồi mới làm `feat/` dựa trên đó.

---

## 📋 PR template

Dùng mẫu dưới đây làm mô tả PR. (Có thể lưu thành `.github/pull_request_template.md`
để GitHub tự điền khi mở PR.)

```markdown
## Mục đích
<!-- PR này giải quyết vấn đề gì? Link issue nếu có: Closes #123 -->

## Thay đổi chính
<!-- Liệt kê theo file/module — bắt buộc với PR Medium/Large -->
- `module-a`: ...
- `module-b`: ...

## Loại thay đổi
- [ ] feat — tính năng mới
- [ ] fix — sửa bug
- [ ] refactor — không đổi behavior
- [ ] docs / chore / test / perf / style

## Kích thước (git diff --stat so với base)
- [ ] Small (< 200) · [ ] Medium (200–500) · [ ] Large (> 500 — đã trao đổi với FSD)

## Kiểm chứng
<!-- Đã test thế nào? Test mới nào được thêm? Edge/failure case nào được cover? -->

## Checklist
- [ ] Branch & commit đúng convention (type khớp prefix branch)
- [ ] Tuân thủ Coding Standards (naming, error handling, no magic numbers, tests)
- [ ] Không gộp refactor lớn với feature trong cùng PR
- [ ] Đã chạy validate cục bộ và không còn CRITICAL/HIGH
```

---

## 🚦 CI gates

Code phải vượt hai tầng cổng trước khi được merge.

### ① Local — pre-push hook (husky)

Hook `.husky/pre-push` chạy trước mỗi lần `git push`:

```bash
node tools/validate-changed-skills.js
```

- Git truyền các ref range đang push qua stdin; script map file đã đổi → thư mục skill
  của chúng, rồi chạy `validate-skills.js` **chỉ trên các skill đó**.
- **Push bị chặn** nếu bất kỳ skill nào đã đổi có finding mức **CRITICAL / HIGH**.

Chạy thủ công khi cần:

| Lệnh | Tác dụng |
|---|---|
| `npm run validate:skills` | Validate toàn bộ skill trong repo |
| `npm run validate:changed` | Validate chỉ các skill đã thay đổi |

### ② Remote — GitHub Actions "AI Review"

Workflow `.github/workflows/ai-review.yml` chạy khi PR được **opened / synchronize /
reopened**:

- `runs-on: ubuntu-latest`, Node 20, `npm ci`.
- Chạy `node scripts/review.js` (AI review), dùng secret `GITHUB_TOKEN` và
  `GEMINI_API_KEY`.
- Quyền: `contents: read`, `pull-requests: write` — review được post ngược lại vào PR.

> [!TIP]
> Vì pre-push đã chặn CRITICAL/HIGH ở máy local, hãy fix sạch trước khi push để không
> tốn một vòng CI. `synchronize` nghĩa là **mỗi lần push thêm commit vào PR** đều
> trigger lại AI Review.

---

