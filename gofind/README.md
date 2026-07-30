# Đánh giá GoFind Method

Ba tài liệu, trả lời hai câu hỏi khác nhau. Đọc mục này trước để biết cần cái nào.

| Câu hỏi | Dùng file | Góc nhìn |
|---|---|---|
| Từng skill có làm đúng việc của nó không? Hỏng ở đâu? | `3-Skill-rubric.md` | Hộp trắng — soi artifact từng bước |
| Cả framework có hiệu quả hơn không dùng gì không? | `1-Criteria.md` + `2-How-to-test.md` | Hộp đen — so kết quả cuối với baseline |

Hai câu trả lời **có thể ngược chiều nhau**. Năm skill đều đạt mà framework vẫn không hơn chat thường — nghĩa là các bước làm đúng nhưng cả quy trình không đáng công. Ngược lại, framework hiệu quả rõ mà một skill trượt cổng — nghĩa là đang hiệu quả nhờ may, và sẽ hỏng khi gặp ca khác. Cần cả hai.

---

## Chạy theo thứ tự nào

**Chạy `3-Skill-rubric.md` trước.** Nó rẻ hơn, chạy trên tình huống dựng sẵn thay vì một đề bài thật, và khi hỏng thì chỉ đúng chỗ hỏng. Có skill nào trượt mục `[PF]` thì sửa trước — đo hiệu quả tổng thể của một framework đang thủng cổng chỉ ra một con số không dùng được.

**Rồi mới chạy `1-Criteria.md` + `2-How-to-test.md`.** Bộ này tốn một buổi: một đề bài làm ba lần theo ba cách, chấm mù, rồi sửa tới khi thật dùng được.

| File | Vai trò | Cách dùng |
|---|---|---|
| `1-Criteria.md` | Bảng tiêu chí và công thức kết luận | Bảng để **điền**. Không tự nó nói cách lấy số |
| `2-How-to-test.md` | Quy trình 12 bước lấy số | Làm theo thứ tự. Mỗi bước ghi rõ điền vào ô nào của `1-Criteria.md` |

Hai file này đi cặp, không dùng riêng được.

---

## Biết trước những gì bộ này không nói

- **Phủ 18/23 tiêu chí.** Năm tiêu chí còn lại cần người thứ hai hoặc cần thời gian trôi qua — liệt kê ở §8 của `1-Criteria.md`, không đo được trong một buổi.
- **Một lần chạy mỗi cách.** Chênh lệch dưới 15% không kết luận được. Bảng kết luận ở §5 đã có dòng lọc này đứng đầu.
- **Nhóm 3 (tính đều) cần thêm một lần chạy** ở Bước 10. Bỏ bước đó thì kết luận chỉ đúng một nửa, vì "đều hơn" là một trong hai mục tiêu đã chọn.
- **Đọc điểm sản phẩm trước, điểm mỗi giờ sau.** Điểm mỗi giờ là thước của "nhanh hơn", không phải mục tiêu đã chọn.

---

## Ghi chú

`1-Criteria.md` viết ở mức chung, dùng lại được cho framework khác. `3-Skill-rubric.md` bám sát tên skill và artifact của GoFind, nên đổi framework là phải viết lại.

Bước 11 của `2-How-to-test.md` (kiểm update/uninstall có giữ file người dùng) nằm ngoài phạm vi đánh giá hiệu quả — nó là lỗi mất dữ liệu của phần cài đặt, phải sửa trước khi phát hành, độc lập với mọi kết luận ở đây.
