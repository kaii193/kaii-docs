# Tiêu chí đánh giá một AI Framework

Dùng cho GoFind Method, và dùng được cho framework tương tự.

---

## 1. Chọn kiểu hiệu quả muốn kiểm tra

Framework giúp được theo bốn cách. Mỗi cách đo bằng thước khác nhau. Chọn trước, vì framework "nhanh hơn" mà đo bằng thước "tốt hơn" sẽ trông như thất bại.

| Kiểu | Nghĩa | Thước đo chính | Chọn |
|---|---|---|:---:|
| Nhanh hơn | cùng kết quả, ít giờ hơn | giờ làm | ☐ |
| Tốt hơn | cùng giờ, kết quả tốt hơn | điểm sản phẩm | ☐ |
| Đều hơn | bớt may rủi, lần nào cũng tạm ổn | chênh lệch giữa các lần chạy | ☐ |
| Đỡ phụ thuộc người giỏi | người mới làm được việc trước cần người kinh nghiệm | so người mới có framework với người cũ không có | ☐ |

GoFind theo README đang nhắm **tốt hơn** và **đều hơn**. Đó cũng là hai kiểu khó chứng minh nhất.

---

## 2. Ba cách làm để so sánh

Cùng một đề bài, làm ba lần theo ba cách:

| Cách | Làm gì |
|---|---|
| **Đầy đủ** | GoFind trọn vẹn: `gf help`, Phase 1→4, agent, workflow, các điểm dừng HALT |
| **Template** | Chỉ lấy file template của nó, tự điền bằng chat thường. Không agent, không workflow |
| **Không dùng gì** | Chat tự do với cùng model |

Có cách **Template** ở giữa là để tách ra: giá trị nằm ở mấy cái template, hay nằm ở agent và workflow.

---

## 3. Cách chấm điểm sản phẩm

Chấm trên phần mềm chạy được, không chấm trên tài liệu. Thang 100.

| Phần | Điểm | Cách tính |
|---|---|---|
| Làm đúng những gì đề bài yêu cầu | 60 | 60 × (số mục đạt ÷ tổng số mục) |
| Bắt được yêu cầu không ai nói ra | 20 | 0 không thấy · 10 có nêu nhưng không xử lý · 20 xử lý được |
| Không có lỗi nặng | 20 | 20 ÷ (1 + số lỗi nặng) |

Lỗi nặng = mất dữ liệu, sai kết quả mà không báo, hoặc chặn luồng chính.

**Trần cứng:** có lỗi làm **mất dữ liệu** thì điểm sản phẩm tối đa 50, bất kể phần còn lại. Không có trần này thì một sản phẩm xoá mất dữ liệu người dùng vẫn chấm được 85/100 — con số đó vô nghĩa. Cùng lý do với quyền phủ quyết của Pass/Fail trong `3-Skill-rubric.md`.

---

## 4. Sáu nhóm tiêu chí

Cột **Lấy ở** trỏ tới bước tương ứng trong `2-How-to-test.md`. Ô ghi **§8** là tiêu chí không đo được trong một buổi — đã điền sẵn "chưa đo", đừng để trống.

### Nhóm 1 — Sản phẩm cuối

| Tiêu chí | Đo bằng | Lấy ở | Đầy đủ | Template | Không dùng gì |
|---|---|---|---|---|---|
| Làm đúng yêu cầu | % mục đạt | Bước 5 | | | |
| Bắt được yêu cầu ẩn | 0 / 10 / 20 | Bước 5 | | | |
| Lỗi nặng | số lỗi | Bước 5 | | | |
| **Điểm sản phẩm** | /100 | Bước 5 | | | |

### Nhóm 2 — Chi phí

| Tiêu chí | Đo bằng | Lấy ở | Đầy đủ | Template | Không dùng gì |
|---|---|---|---|---|---|
| Giờ làm | giờ, trừ lúc chờ AI | Bước 3 | | | |
| Giờ sửa thêm | giờ | Bước 6 | | | |
| **Tổng giờ** | giờ làm + giờ sửa | Bước 9 | | | |
| Tiền | USD hoặc token | Bước 3 | | | |
| Số lượt qua lại với AI | số turn | Bước 3 | | | |
| Phí mở màn | giờ trước dòng code đầu tiên | Bước 3 | | | |

### Nhóm 3 — Tính đều

Đây là lý do tồn tại của một framework: biến kết quả may rủi thành kết quả lặp lại được. Model đơn lẻ đã đủ giỏi để làm tốt một lần.

| Tiêu chí | Đo bằng | Lấy ở | Đầy đủ | Template | Không dùng gì |
|---|---|---|---|---|---|
| Làm lại lần hai có ra tương đương | chênh lệch điểm giữa 2 lần **cùng một đề bài** | Bước 10 | | | |
| Output có đúng khuôn | % tài liệu đủ mục theo template | Bước 3b | | | |
| Hai người khác nhau có ra tương đương | chênh lệch điểm giữa 2 người | **§8** | chưa đo | chưa đo | chưa đo |

**Cùng đề, không phải đề khác.** Hai phép đo này khác nhau: cùng đề đo *độ ổn định giữa các lần chạy*, đề khác đo *khả năng khái quát*. Mục tiêu đã chọn ở §1 là "đều hơn", nên phải là cùng đề. Đổi lại phải chấp nhận lần hai bị thổi lên vì đã quen bài — ghi nhận điều đó khi đọc kết quả, đừng bỏ qua.

Chỉ đo được nếu chạy lại lần hai (Bước 10 của `2-How-to-test.md`). Không chạy lại thì ghi "chưa đo", và kết luận ở §5 chỉ là dấu hiệu ban đầu.

### Nhóm 4 — Con người

| Tiêu chí | Đo bằng | Lấy ở | Đầy đủ | Template | Không dùng gì |
|---|---|---|---|---|---|
| Ai ra quyết định | trong 20 quyết định chính, bao nhiêu do bạn nêu trước | Bước 3b | | | |
| Bạn có bắt được lỗi của AI | lỗi cài vào tài liệu sống được bao xa | Bước 7 | | | |
| Bấm qua điểm dừng cho nhanh | số lần duyệt dưới 15 giây | Bước 3 | | | |
| Người mới bao lâu thì làm được | giờ tới sản phẩm đầu tiên dùng được | **§8** | chưa đo | chưa đo | chưa đo |
| Bạn có giỏi lên | làm lại một bài không dùng framework sau 4 tuần, so trước | **§8** | chưa đo | chưa đo | chưa đo |

### Nhóm 5 — Bàn giao và duy trì

| Tiêu chí | Đo bằng | Lấy ở | Đầy đủ | Template | Không dùng gì |
|---|---|---|---|---|---|
| Người khác đọc tài liệu có làm tiếp được | số câu họ phải hỏi lại | **§8** | chưa đo | chưa đo | chưa đo |
| Sau 30 ngày còn dùng không | có/không | **§8** | chưa đo | chưa đo | chưa đo |
| Có bỏ bước không | số phase bị nhảy qua | Bước 3b | | | |

### Nhóm 6 — Rủi ro

| Tiêu chí | Đo bằng | Lấy ở | Đầy đủ | Template | Không dùng gì |
|---|---|---|---|---|---|
| Lần tệ nhất tệ đến đâu | mô tả output xấu nhất | Bước 3b | | | |
| Framework có tự tin sai | số lần khẳng định chắc chắn một điều sai | Bước 3b | | | |

---

## 5. Con số kết luận

**Điểm mỗi giờ = điểm sản phẩm ÷ tổng giờ**

| | Điểm sản phẩm | Tổng giờ | Điểm mỗi giờ |
|---|---|---|---|
| Đầy đủ | | | |
| Template | | | |
| Không dùng gì | | | |

**Đọc điểm sản phẩm trước, điểm mỗi giờ sau.** §1 đã chọn mục tiêu là "tốt hơn" và "đều hơn" — điểm mỗi giờ là thước của "nhanh hơn". Nó có ích để phát hiện framework ăn quá nhiều giờ, nhưng không phải con số trả lời câu hỏi chính. Lấy nó làm số dẫn đầu là rơi đúng cái bẫy §1 cảnh báo.

Đọc theo thứ tự, dừng ở dòng đầu tiên khớp:

| Điều kiện | Kết luận |
|---|---|
| Chênh lệch **điểm mỗi giờ** giữa Đầy đủ và cách gần nhất dưới 15% | **Không phân biệt được.** Một lần chạy không đủ — không đọc tiếp các dòng dưới |
| Đầy đủ thấp hơn Không dùng gì | **Không hiệu quả.** Framework lấy nhiều hơn phần nó tạo ra |
| Đầy đủ không hơn Template | **Giá trị nằm ở template, không ở agent và workflow.** Dùng template thôi |
| Đầy đủ cao nhất, nhưng điểm sản phẩm không hơn Không dùng gì | **Hiệu quả về thời gian, không về chất lượng.** Chỉ dùng khi cần nhanh |
| Đầy đủ cao nhất cả hai | **Hiệu quả.** Ghi rõ cao hơn bao nhiêu phần trăm |

Trừ dòng đầu, "cao hơn / thấp hơn" đều nói về **điểm mỗi giờ**; riêng hai dòng cuối có nhắc điểm sản phẩm thì lấy đúng điểm sản phẩm.

Dòng lọc 15% phải đứng đầu, không đứng cuối. Bốn dòng dưới đã phủ kín mọi trường hợp, nên nếu để nó ở cuối thì không bao giờ tới lượt và mọi chênh lệch nhiễu đều bị đọc thành kết luận thật.

**Thêm một dòng nữa nếu đã chạy Bước 10:**

| Điều kiện | Kết luận |
|---|---|
| Chênh lệch điểm sản phẩm giữa 2 lần cùng đề trên 15 điểm | **Chưa đều.** Dù kết luận trên có đẹp, framework chưa giữ được lời hứa cốt lõi — kết quả vẫn còn may rủi |

Chưa chạy Bước 10 thì ghi "Nhóm 3: chưa đo" ngay cạnh kết luận. Không được lặng lẽ bỏ qua: "đều hơn" là một trong hai mục tiêu đã chọn ở §1, kết luận thiếu nó chỉ đúng một nửa.

---

## 6. Phải có mốc so sánh

Mọi tiêu chí trên chỉ có nghĩa khi so với một mốc. Ba cách lấy mốc:

| Cách | Điểm mạnh | Điểm yếu |
|---|---|---|
| Làm lại cùng bài mà không dùng framework | so trực tiếp được | lần sau đã quen bài, kết quả bị thổi lên |
| Lấy số của 3–5 task đã làm trước khi có framework | rẻ nhất, dữ liệu có sẵn | task không giống nhau |
| So Đầy đủ với Template | tách được phần nào tạo giá trị | không có mốc "không dùng gì" |

---

## 7. Năm dấu hiệu framework không hiệu quả

Kiểm tra được nhanh, và thường cho câu trả lời trước cả khi đo xong.

- ☐ Sản phẩm cuối không tốt hơn, chỉ có tài liệu dày hơn
- ☐ Bỏ phần lập kế hoạch đi mà kết quả vẫn như cũ
- ☐ Bạn bấm qua các điểm dừng trong dưới 15 giây
- ☐ Lỗi bạn cố tình cài vào tài liệu đi hết tới code mà không ai chặn
- ☐ Chỉ dùng mấy cái template cũng cho kết quả tương đương

Dấu hiệu cuối là dấu hiệu đáng chú ý nhất. Nó không nói framework tệ. Nó nói phần giá trị nằm ở 20% đơn giản nhất, và bản tiếp theo nên gọn hơn chứ không nên nhiều hơn.

---

## 8. Tiêu chí phải để lại đo sau

Năm tiêu chí dưới đây nằm trong §4 nhưng **không đo được trong một buổi test** — cần người thứ hai, hoặc cần thời gian trôi qua. `2-How-to-test.md` không có bước nào cho chúng, và đó là cố ý.

| Tiêu chí | Thuộc nhóm | Cần gì để đo | Sớm nhất khi nào |
|---|---|---|---|
| Hai người khác nhau có ra tương đương | 3 — Tính đều | người thứ hai chạy cùng đề | có người thứ hai |
| Người mới bao lâu thì làm được | 4 — Con người | một người chưa từng dùng framework | có người mới |
| Bạn có giỏi lên | 4 — Con người | làm lại một bài không dùng framework | sau 4 tuần |
| Người khác đọc tài liệu có làm tiếp được | 5 — Bàn giao | người thứ hai đọc tài liệu và làm tiếp | có người thứ hai |
| Sau 30 ngày còn dùng không | 5 — Bàn giao | thời gian trôi qua | sau 30 ngày |

Để trống trong bảng §4 thì đọc thành "đo rồi mà không có kết quả". Ghi thẳng **"chưa đo — xem §8"** vào ô đó.

Ba tiêu chí còn lại từng bị bỏ sót (output có đúng khuôn; ai ra quyết định; có bỏ bước không) và hai tiêu chí Nhóm 6 nay đã có bước thực thi — xem Bước 3b của `2-How-to-test.md`.

**Ngoài phạm vi bộ tiêu chí này:** Bước 11 của `2-How-to-test.md` kiểm update/uninstall có giữ file người dùng không. Đó là lỗi mất dữ liệu của phần cài đặt, phải sửa trước khi phát hành, nhưng không nói gì về hiệu quả của phương pháp — nên không có nhóm tiêu chí tương ứng ở §4 và không vào công thức kết luận ở §5.

---

## 9. Bộ tiêu chí này không trả lời được

| Câu hỏi | Vì sao |
|---|---|
| Hiệu quả trên dự án nhiều tháng | đo trên một đề bài vài giờ |
| Hiệu quả sau khi hết hiệu ứng mới lạ | đo ngay lúc mới dùng |
| Hiệu quả với người trình độ khác | một người chạy |
| Hiệu quả trên dự án có sẵn codebase lớn | đề bài làm từ đầu |
| Chênh lệch giữa các IDE khác nhau | một công cụ |