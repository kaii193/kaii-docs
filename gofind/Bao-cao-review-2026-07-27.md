# Báo cáo review bộ tài liệu đánh giá GoFind

Ngày: 27/07/2026 · Phạm vi: 3 file trong `gofind/`

---

## Tóm tắt

Bộ tài liệu về cơ bản chắc, và có vài thiết kế tốt hiếm gặp — nhánh Template để tách nguồn giá trị, thứ tự chạy cố ý bất lợi cho chính giả thuyết, lỗi cài mù bằng `shuf`. Vấn đề không nằm ở ý tưởng mà ở **chỗ nối**: hai file mâu thuẫn nhau ở một định nghĩa, một bảng quyết định có dòng không bao giờ chạy tới, và một phần ba số tiêu chí không có bước nào đo chúng.

| | Trước | Sau |
|---|---|---|
| Tham chiếu gãy | 3 | 0 |
| Mâu thuẫn giữa các file | 2 | 0 |
| Tiêu chí có bước đo | 14 / 24 | **18 / 23** |
| Tiêu chí thiếu mà không ai biết | 10 | 0 — còn 5, khai báo rõ ở §8 |
| Hạng mục rubric không rõ loại | 20 / 35 | 0 |
| Công thức tính điểm còn thiếu | 2 | 0 |

Đã sửa **23 vấn đề**. Thêm 1 bước đo mới, 1 mục mới, 1 file README.

---

## 1. Ba lỗi làm tài liệu chạy sai

Không phải góp ý phong cách — ba lỗi này khiến người làm theo sẽ ra kết quả sai.

### 1.1 Bảng kết luận có một dòng không bao giờ chạy tới

`1-Criteria.md` §5 ghi *"đọc theo thứ tự, dừng ở dòng đầu tiên khớp"*, và đặt dòng lọc nhiễu ở **cuối**:

> | Chênh lệch dưới 15% | Không phân biệt được. Một lần chạy không đủ |

Bốn dòng phía trên đã phủ kín mọi trường hợp, nên dòng này không bao giờ tới lượt. Hậu quả: mọi chênh lệch nhiễu đều được đọc thành kết luận thật, dù cả bộ tài liệu chỉ chạy một lần mỗi cách.

Đáng chú ý là `2-How-to-test.md` làm **đúng** — lọc ở Bước 8, kết luận ở Bước 9. Hai file đang mâu thuẫn về thứ tự.

**Đã sửa:** đưa dòng lọc lên đầu bảng, nói rõ "không đọc tiếp các dòng dưới", và chỉ rõ chênh lệch của đại lượng nào (điểm mỗi giờ — trước đây không ghi).

### 1.2 Hai file định nghĩa "tính đều" ngược nhau

| Nguồn | Nói gì |
|---|---|
| `1-Criteria.md` Nhóm 3 | chênh lệch điểm giữa 2 lần **cùng đề** |
| `2-How-to-test.md` Bước 10 | chạy lại trên **một đề bài khác** cùng độ khó |

Đây là hai phép đo khác nhau: cùng đề đo *độ ổn định giữa các lần chạy*, khác đề đo *khả năng khái quát*. Làm theo Bước 10 rồi điền vào ô Nhóm 3 là điền số của phép đo này vào chỗ của phép đo kia.

**Đã sửa:** chốt **cùng đề**, vì mục tiêu đã chọn ở §1 là "đều hơn". Ghi rõ cái giá phải trả — lần hai đã quen bài nên chỉ đọc được theo một chiều: lệch nhiều thì kết luận "chưa đều" là chắc, lần hai cao hơn thì chưa nói lên gì.

### 1.3 Lỗi cài cố ý làm hỏng chính phép so sánh mù

Bước 3 chèn một yêu cầu sai vào tài liệu, **chỉ ở lần Đầy đủ**. Nếu nó đi tới code — đúng cái ta muốn phát hiện — nó biến thành mục checklist không đạt hoặc lỗi nặng, và trừ điểm lần Đầy đủ ở Bước 5. Template và Không dùng gì không chịu khoản trừ nào. Không chỗ nào trong Bước 5 hay Bước 8 gỡ khoản này.

Kết quả: bài test chống lại GoFind hai lần. Lần thứ nhất cố ý và hợp lý (thứ tự chạy ở Bước 3 đặt hiệu ứng quen bài về phía hai baseline). Lần thứ hai vô tình, và không ai biết.

**Đã sửa:** thêm mục "Gỡ thiên lệch do chính lỗi cài cố ý" vào Bước 7 — tính lại điểm Đầy đủ sau khi loại thiệt hại truy được về lỗi cài, báo cáo **cả hai số**. Bước 9 lấy số đã gỡ, vì chỉ số đó cùng điều kiện với hai cách kia.

---

## 2. Khoảng trống bao phủ

`2-How-to-test.md` tự giới thiệu là "cách test theo bộ tiêu chí" nhưng chỉ phủ 14/24 tiêu chí, và không nói mình bỏ cái gì.

| Nhóm | Trước | Sau | Cách xử lý |
|---|:---:|:---:|---|
| 1 — Sản phẩm cuối | 5/5 | 4/4 | bỏ 1 dòng trùng với Nhóm 2 |
| 2 — Chi phí | 6/6 | 6/6 | — |
| 3 — Tính đều | 1/3 | 2/3 | thêm bước đo "output đúng khuôn"; 1 hoãn |
| 4 — Con người | 2/5 | 3/5 | thêm bước đo "ai ra quyết định"; 2 hoãn |
| 5 — Bàn giao | **0/3** | 1/3 | thêm bước đo "có bỏ bước không"; 2 hoãn |
| 6 — Rủi ro | **0/2** | 2/2 | thêm bước đo cả hai |
| **Tổng** | **14/24** | **18/23** | 5 tiêu chí hoãn, khai báo rõ |

Nhóm 5 và 6 trước đây hoàn toàn không có bước thực thi.

**Đã thêm Bước 3b — Sổ ghi trong lúc làm.** Bốn tiêu chí này chỉ ghi được ngay lúc đang làm, xong việc rồi ngồi nhớ lại thì số ra sai: ai ra quyết định (gạch B/A cho 20 quyết định), số lần bỏ bước, số lần AI tự tin sai, đoạn output tệ nhất. Cộng một mục đo sau khi chạy xong: % tài liệu đủ mục theo template.

**Đã thêm §8 — Tiêu chí phải để lại đo sau.** Năm tiêu chí cần người thứ hai hoặc cần thời gian trôi qua (30 ngày, 4 tuần). Chúng đã được điền sẵn "chưa đo" trong bảng §4 thay vì để trống — ô trống đọc thành "đo rồi mà không có kết quả".

**Chiều ngược lại:** Bước 11 (kiểm update/uninstall giữ file) không có nhóm tiêu chí tương ứng. Nó hợp lệ nhưng nằm ngoài phạm vi đánh giá hiệu quả — nay được §8 ghi nhận rõ.

---

## 3. Thước đo lệch mục tiêu

`1-Criteria.md` §1 bắt người dùng chọn kiểu hiệu quả muốn kiểm, và cảnh báo: *framework "nhanh hơn" mà đo bằng thước "tốt hơn" sẽ trông như thất bại*. Rồi tự chọn mục tiêu là **tốt hơn** và **đều hơn**.

Nhưng con số kết luận duy nhất ở §5 là **điểm mỗi giờ** — thước của "nhanh hơn". Đúng cái bẫy §1 vừa cảnh báo. Và Nhóm 3 (tính đều), thứ §4 gọi là "lý do tồn tại của một framework", không xuất hiện ở bất kỳ dòng nào trong bảng kết luận — trong khi `2-How-to-test.md` Bước 10 lại mở đầu bằng "**muốn** có Nhóm 3 thì làm thêm một lần".

Nghĩa là: lời hứa cốt lõi vừa là tuỳ chọn, vừa không nằm trong công thức kết luận.

**Đã sửa:**
- §5 nói rõ đọc điểm sản phẩm trước, điểm mỗi giờ sau, kèm lý do
- Thêm một dòng kết luận cho Nhóm 3: chênh lệch trên 15 điểm → "chưa đều", dù kết luận chính có đẹp
- Bước 10 không còn là tuỳ chọn; bỏ thì bắt buộc ghi "Nhóm 3: chưa đo" cạnh kết luận

---

## 4. Rubric skill — chấm điểm chưa tính được

`3-Skill-rubric.md` §7 nói "điểm chất lượng quy về 100" nhưng **không có công thức**, và số mục 1–5 mỗi skill khác nhau (3, 4, 4, 4, 5). Thêm nữa, 3 mục chất lượng chung ở §2 không rõ có tính vào hay không. Ngưỡng 80/65 vì vậy mang nghĩa khác nhau ở mỗi skill.

Song song, 20/35 hạng mục không rõ thuộc loại nào:
- Có mục vừa gắn ngưỡng vừa gắn thang: *"Coverage ≥ 0.90 (1–5)"* — ngưỡng thì nhị phân, thang thì liên tục, mâu thuẫn nội tại
- Có mục không gắn gì cả: *"AC Met ≥ 0.95"*, *"Recall ≥ 0.90"* — Recall 0.88 là trượt, hay là điểm 4?

**Đã sửa:**

| Vấn đề | Cách xử lý |
|---|---|
| Thiếu công thức | `điểm = tổng ÷ (số mục × 5) × 100`, kèm bảng số mục từng skill |
| Không rõ mục chung có tính không | Tính — 3 mục chất lượng chung áp cho cả 5 skill |
| Ngưỡng lẫn thang | Ngưỡng là **mốc của điểm 4**, kèm bảng quy đổi 5 bậc. Không ngưỡng nào biến mục 1–5 thành trượt |
| 20 mục không rõ loại | Gắn nhãn `[PF]` hoặc `(1–5)` cho toàn bộ 35 mục, mỗi mục đúng một nhãn |
| 3 mục Pass/Fail chung trùng với mục riêng | Ghi rõ là nguyên tắc, không chấm hai lần |
| ⑤ chỉ có 1 cổng cứng, ① có 4 | Thêm 2 mục `[PF]` dạng hợp đồng cho ⑤ |
| "3–5 lần" vs "5 lần" | Chốt 5, khớp metric Consistency |
| "(Mục 8)" — không tồn tại | → Mục 6 |

Về ⑤: lưới chắn cuối cùng trước merge mà chỉ có một quyền phủ quyết là quá mỏng. Hai mục thêm vào là hành vi nhị phân đúng kiểu hợp đồng, không phải metric — không tự sửa code khi đang review, và không bỏ sót file nào trong diff.

---

## 5. Thang điểm sản phẩm

Hai chỗ trong `1-Criteria.md` §3:

**Lỗi nặng không có quyền phủ quyết.** Công thức `20 ÷ (1 + số lỗi nặng)` nghĩa là sản phẩm **làm mất dữ liệu** vẫn đạt tới 85/100. Trong khi `3-Skill-rubric.md` §7 lập luận rất đúng rằng cổng hỏng phải là điều kiện cứng, không bù trừ bằng điểm đẹp chỗ khác. Hai file theo hai triết lý chấm khác nhau.
→ **Đã thêm trần cứng:** có lỗi mất dữ liệu thì điểm sản phẩm tối đa 50.

**Một câu nhị phân nắm 20% tổng điểm.** "Bắt được yêu cầu ẩn — 20 nếu được, 0 nếu không". Với n=1 mỗi nhánh, riêng ô này lật được thứ hạng.
→ **Đã chia ba bậc:** 0 không thấy / 10 có nêu nhưng không xử lý / 20 xử lý được.

---

## 6. Các sửa nhỏ

| Vấn đề | Sửa |
|---|---|
| `1-CRITERIA.md` không tồn tại (file tên `Criteria.md`) — 2 chỗ | Đổi tên file có đánh số, sửa tham chiếu |
| Bước 6 cho ghi `>4` giờ, Bước 9 lại cần số để tính | Quy ước `>4` = 6 giờ, đánh dấu `*`; kết luận lật chiều khi đổi 6→5 hoặc 8 thì ghi "không phân biệt được" |
| Bước 5 chỉ có quy tắc đối chiếu cho lỗi, không có cho số mục đạt | Thêm bảng 4 dòng; mục "không kiểm tra được" bắt buộc tự chạy để chốt |
| "Giờ sửa thêm" nằm ở cả Nhóm 1 và Nhóm 2 | Bỏ khỏi Nhóm 1 |
| Không biết tiêu chí nào lấy số ở bước nào | Thêm cột **Lấy ở** vào cả 6 bảng nhóm |
| Ba file không nhắc tới nhau, không có điểm vào | Thêm `README.md` |

---

## 7. Quyết định thiết kế — cần xác nhận

Bảy chỗ dưới đây tôi phải chọn một phương án để tài liệu tính được. Đều là quyết định của bạn, sửa lại dễ:

| # | Quyết định | Đã chọn | Phương án khác |
|---|---|---|---|
| 1 | Nhóm 3 đo thế nào | cùng đề | khác đề — nhưng khi đó phải đổi tên tiêu chí thành "khả năng khái quát" |
| 2 | Trần điểm khi mất dữ liệu | 50 | bỏ trần, hoặc đặt 0 |
| 3 | Yêu cầu ẩn | 0 / 10 / 20 | giữ nhị phân 0 / 20 |
| 4 | `>4` giờ quy thành | 6 | 5, 8, hoặc loại nhánh đó khỏi so sánh |
| 5 | Ngưỡng metric trong rubric | mốc của điểm 4 | mốc của điểm 3, hoặc biến thành `[PF]` |
| 6 | 3 mục chất lượng chung | tính vào điểm 100 mọi skill | chấm riêng, không cộng |
| 7 | Ngưỡng "chưa đều" | 15 điểm | 10 hoặc 20 |

Quyết định 1 và 5 ảnh hưởng nhiều nhất tới kết quả cuối.

---

## 8. Còn lại — biết mà chưa xử lý

Không phải lỗi tài liệu, mà là giới hạn của thiết kế bài test. Nêu ra để không ai đọc kết quả quá tay:

- **n = 1 mỗi nhánh.** Dòng lọc 15% giúp được phần nào nhưng không thay được cỡ mẫu. Kết luận từ một buổi test là dấu hiệu, không phải bằng chứng — `1-Criteria.md` §9 đã nói thẳng điều này.
- **Chấm mù là tự chấm mù.** Người trộn X/Y/Z chính là người làm cả ba lần. Đã giảm rủi ro bằng `grep` chống rò tên và đếm số từ ở Bước 8, nhưng người thứ hai vẫn tốt hơn.
- **Năm tiêu chí ở §8 vẫn chưa đo được** cho tới khi có người thứ hai hoặc đủ 30 ngày.
- **`3-Skill-rubric.md` chưa có quy trình chạy** tương đương `2-How-to-test.md` — Mục 6 mô tả golden scenarios cần dựng nhưng chưa có file kịch bản thật. Đây là việc đáng làm tiếp theo.

---

## Phụ lục — thay đổi theo file

| File | Thay đổi |
|---|---|
| `README.md` | **mới** — điểm vào, phân biệt hai bộ đánh giá, thứ tự chạy, giới hạn cần biết trước |
| `1-Criteria.md` | §3 trần cứng + chia bậc yêu cầu ẩn · §4 thêm cột Lấy ở, bỏ dòng trùng, chốt định nghĩa Nhóm 3 · §5 đảo dòng lọc lên đầu, thêm khung đọc kết quả, thêm dòng Nhóm 3 · **§8 mới** · §8 cũ → §9 |
| `2-How-to-test.md` | sửa 2 tham chiếu gãy · thêm dòng khai báo độ phủ · **Bước 3b mới** · Bước 5 bảng đối chiếu · Bước 6 quy ước `>4` · **Bước 7 gỡ thiên lệch lỗi cài** · Bước 9 dùng số đã gỡ · Bước 10 chuyển sang cùng đề, không còn tuỳ chọn |
| `3-Skill-rubric.md` | sửa tham chiếu Mục 6 · chốt 5 lần chạy · bảng quy metric về thang 1–5 · làm rõ vai trò 6 tiêu chí chung · gắn nhãn 35 hạng mục · thêm 2 `[PF]` cho ⑤ · **công thức quy về 100** · liên kết tới hai file kia |

Ba file đã đổi tên có đánh số theo thứ tự dùng: `Criteria.md` → `1-Criteria.md`, `How-to-test.md` → `2-How-to-test.md`, `Skill-rubric.md` → `3-Skill-rubric.md`.
