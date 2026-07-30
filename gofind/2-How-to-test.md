# Cách test GoFind Method theo bộ tiêu chí

Làm theo thứ tự. Mỗi bước ghi rõ làm gì và điền vào đâu trong `1-Criteria.md`.

Quy trình này phủ **18/23 tiêu chí** của `1-Criteria.md`. Năm tiêu chí còn lại cần người thứ hai hoặc cần thời gian trôi qua — xem §8 của file đó.

---

## Bước 1 — Viết một đề bài

Một đề bài duy nhất, dùng chung cho cả ba cách làm.

Yêu cầu:
- Dài 3–6 câu
- Cố ý để lại 2 chỗ mơ hồ
- Chứa **1 yêu cầu không nói thẳng ra** — ví dụ phân quyền, xử lý lỗi, hoặc dữ liệu rỗng
- Làm xong được trong 4 giờ
- Người khác không đọc đề vẫn kiểm tra được kết quả

| Mục | Nội dung |
|---|---|
| Đề bài | |
| Chỗ mơ hồ 1 | |
| Chỗ mơ hồ 2 | |
| Yêu cầu ẩn | |

Lưu lại. Không sửa sau khi Bước 3 bắt đầu.

---

## Bước 2 — Lập bộ kiểm tra, trước khi có dòng code nào

Đây là bước quyết định độ chính xác của cả bài test. Lập xong thì khoá lại, không sửa nữa.

Mở một chat mới, dán:

```
Đây là đặc tả một task phát triển. Chưa có code nào để xem.

[dán đề bài]

Viết checklist kiểm tra gồm:
1. Mỗi yêu cầu tường minh trong đặc tả thành một mục
2. Mọi yêu cầu ngầm suy ra được từ đặc tả: phân quyền, xử lý lỗi, dữ liệu rỗng, đầu vào sai
3. 8 trường hợp biên

Mỗi mục viết dạng: hành động cụ thể → kết quả mong đợi cụ thể.
Xuất ra bảng có cột Đạt/Không để trống.
Không suy đoán về cách hiện thực.
```

Lưu thành `test-checklist.md`. **Đếm tổng số mục** — dùng để chấm điểm sản phẩm ở Bước 5.

---

## Bước 3 — Làm ba lần

Thứ tự bắt buộc: **Đầy đủ → Template → Không dùng gì**. Không đảo.

Lý do: làm lại cùng một đề thì lần sau luôn dễ hơn vì đã quen bài. Đặt Đầy đủ lên trước để cái lợi đó rơi vào hai cách sau, tức là chạy ngược lại điều bạn đang kỳ vọng. Nếu Đầy đủ vẫn thắng thì kết quả đáng tin.

Nghỉ ít nhất 2 giờ giữa hai lần. Dừng sau 4 giờ dù chưa xong — phần chưa làm tính là không đạt.

Trong lúc làm, đếm ba thứ:

| Đếm gì | Ghi chú |
|---|---|
| Giờ làm | tạm dừng đồng hồ khi ngồi chờ AI |
| Số lượt qua lại với AI | đếm turn |
| Số lần bấm qua điểm dừng dưới 15 giây | chỉ có ở cách Đầy đủ |

Cuối mỗi lần, ghi thêm: **phí mở màn** (bao nhiêu giờ trôi qua trước khi viết dòng code đầu tiên) và **tiền tốn**.

### Cài một lỗi mà bạn không biết là lỗi nào

Làm ở lần **Đầy đủ**, ngay sau khi tài liệu yêu cầu sinh xong, trước khi sang bước kiến trúc.

Mở chat mới:

```
Đọc đề bài sau và viết 5 "yêu cầu sai lệch" dùng cho một bài kiểm tra chất lượng quy trình.

[dán đề bài]

Mỗi câu phải: nghe hợp lý với người đọc lướt, mâu thuẫn với đề bài hoặc thêm ràng buộc
không ai yêu cầu, dài 1-2 câu, viết theo giọng một dòng trong tài liệu yêu cầu.

Chỉ xuất 5 dòng. Không đánh số, không giải thích.
```

Lưu 5 dòng đó vào một file, rồi chạy:

```bash
shuf -n 1 candidates.txt > planted.txt
cat planted.txt >> path/to/requirements-doc.md
rm candidates.txt
```

Không mở `planted.txt`, không đọc phần cuối tài liệu. Làm tiếp bình thường. Mở ở Bước 7.

---

## Bước 3b — Sổ ghi trong lúc làm

Năm tiêu chí ở Nhóm 3, 4, 5, 6 chỉ ghi được **ngay lúc đang làm**. Xong việc rồi ngồi nhớ lại thì số ra sai. Mở sẵn một file `so-ghi.md` bên cạnh, ghi khi nó xảy ra.

| Ghi gì | Cách ghi | Vào tiêu chí | Nhóm |
|---|---|---|---|
| Ai ra quyết định | Mỗi lần chốt một điều đáng kể (chọn cấu trúc dữ liệu, chia module, chọn thư viện, cách xử lý lỗi), gạch một dòng: **B** nếu bạn nêu trước, **A** nếu AI nêu trước rồi bạn thuận theo. Dừng ở 20 dòng | Ai ra quyết định | 4 |
| Bỏ bước | Mỗi lần nhảy qua một phase hoặc bỏ một điểm dừng, gạch một dòng | Có bỏ bước không | 5 |
| AI tự tin sai | Mỗi lần AI khẳng định chắc chắn một điều mà sau đó hoá ra sai, gạch một dòng kèm 5 từ mô tả | Framework có tự tin sai | 6 |
| Lúc tệ nhất | Chép lại đoạn output tệ nhất gặp trong lần chạy đó, 3–5 dòng | Lần tệ nhất tệ đến đâu | 6 |

Ghi cho **cả ba cách làm**, không chỉ Đầy đủ. Ba cột trong bảng §4 mới so sánh được với nhau. Riêng dòng "bỏ bước" chỉ có nghĩa ở cách Đầy đủ — hai cách kia ghi `—`.

Thêm một mục đo sau khi cả ba lần chạy xong, không cần ghi trong lúc làm:

| Đo gì | Cách đo | Vào tiêu chí | Nhóm |
|---|---|---|---|
| Output có đúng khuôn | Với mỗi tài liệu GoFind sinh ra, đếm số mục bắt buộc theo template có mặt đầy đủ. `% = số mục có ÷ tổng số mục template yêu cầu` | Output có đúng khuôn | 3 |

Chỉ cách Đầy đủ và Template có khuôn để đối chiếu. Cách Không dùng gì ghi `—`.

---

## Bước 4 — Trộn ba kết quả rồi mới chấm

Biết kết quả nào của GoFind là đủ để chấm lệch, kể cả khi bạn cố gắng công bằng.

```bash
mkdir -p grading/{X,Y,Z}
# copy ba kết quả vào X, Y, Z theo thứ tự ngẫu nhiên
# ghi bảng giải mã ra giấy, để riêng
rm -rf grading/*/.git
grep -ril "gofind\|_gf\|GF00\|go find" grading/
```

Lệnh `grep` cuối phải không trả về gì. Còn thì mở file xoá tay rồi chạy lại.

Đếm số từ mỗi tài liệu — dùng ở Bước 8:

```bash
wc -w grading/*/*.md
```

---

## Bước 5 — Chấm điểm sản phẩm

Ba chat riêng, mỗi thư mục một chat. Không dán hai thư mục vào cùng một chat.

```
Bạn kiểm tra một codebase do người khác viết.

Đây là bộ kiểm tra đã được duyệt trước khi codebase này tồn tại:
[dán test-checklist.md]

Đây là code:
[đính kèm grading/X]

Với từng mục: đọc code, xác định Đạt hay Không, dẫn ra file và số dòng làm bằng chứng.
Không xác định được thì ghi "không kiểm tra được" kèm lý do.
Sau bảng, liệt kê mọi lỗi tìm thấy ngoài bộ kiểm tra, xếp theo mức nghiêm trọng.
Không đề xuất cách sửa. Không nhận xét chất lượng code.

Ba dòng cuối, đúng định dạng:
SO_MUC_DAT: <số>
YEU_CAU_AN: <co hoặc khong>
SO_LOI_NANG: <số lỗi làm mất dữ liệu, sai kết quả không báo, hoặc chặn luồng chính>
```

Sau đó **tự chạy thử cả ba** theo `test-checklist.md`. Kết quả chạy thật thắng kết quả agent đọc code, ở cả ba con số:

| Agent nói | Chạy thật cho thấy | Lấy số nào |
|---|---|---|
| Mục này Đạt | chạy không ra kết quả mong đợi | **Không đạt** |
| Mục này Không đạt | chạy ra đúng kết quả mong đợi | **Đạt** |
| Có lỗi nặng | không tái hiện được | **bỏ**, không tính vào số lỗi nặng |
| Không nhắc | bạn gặp lỗi nặng khi chạy | **tính thêm** |

Mục nào agent ghi "không kiểm tra được" thì bắt buộc phải tự chạy để chốt — không được để nguyên. Còn giữ trạng thái mơ hồ ở bước này thì `SO_MUC_DAT` sai, và điểm sản phẩm sai theo.

Tính điểm sản phẩm cho từng thư mục:

```
điểm = 60 × (số mục đạt ÷ tổng số mục)
     + 20 nếu bắt được yêu cầu ẩn
     + 20 ÷ (1 + số lỗi nặng)
```

| Thư mục | Mục đạt / tổng | Yêu cầu ẩn | Lỗi nặng | **Điểm sản phẩm** |
|---|---|---|---|---|
| X | | | | |
| Y | | | | |
| Z | | | | |

---

## Bước 6 — Sửa cho tới khi thật sự dùng được

Đây là con số hay bị bỏ qua nhất, và nó đổi hẳn kết luận. Xong trong 2 giờ mà phải sửa thêm 3 giờ thì tốn 5 giờ, không phải 2.

**Dùng được** = mọi mục trong bộ kiểm tra đạt, yêu cầu ẩn được xử lý, không còn lỗi nặng.

Với từng thư mục, tự sửa cho tới khi đạt, bấm giờ. Chỉ sửa, không viết lại từ đầu. Vượt 4 giờ mà chưa đạt thì dừng và ghi `>4`.

**Quy ước cho `>4`:** ở Bước 9 tính là **6 giờ**, và đánh dấu `*` vào ô đó. Không có quy ước thì Tổng giờ và Điểm mỗi giờ không tính được. Con số 6 là ước lượng thận trọng, không phải số đo — nên nếu kết luận cuối cùng lật chiều khi đổi 6 thành 5 hoặc 8, thì kết luận đó chưa đủ chắc, ghi "không phân biệt được".

| Thư mục | Giờ sửa thêm | Đạt được không |
|---|---|---|
| X | | |
| Y | | |
| Z | | |

---

## Bước 7 — Mở bảng giải mã và kiểm lỗi cài cố ý

Giờ mới mở. Đổi X/Y/Z về Đầy đủ / Template / Không dùng gì trong mọi bảng.

Kiểm lỗi cài cố ý, chat mới:

```
Đây là một tài liệu yêu cầu. Đối chiếu với đề bài gốc bên dưới.

[dán tài liệu yêu cầu của lần Đầy đủ]
[dán đề bài]

Liệt kê mọi yêu cầu có trong tài liệu nhưng không có trong đề bài và không được ghi là giả định.
Trích nguyên văn từng mục.
```

Mở `planted.txt`, đối chiếu.

| Câu hỏi | Trả lời |
|---|---|
| Lỗi đã chèn là gì | |
| Trong lúc làm, có điểm dừng nào chặn nó không | |
| Nó có vào tài liệu kiến trúc không | |
| Nó có vào code không | |

### Gỡ thiên lệch do chính lỗi cài cố ý

Lỗi cài cố ý **chỉ chèn vào lần Đầy đủ**. Nếu nó đi tới code — tức là đúng cái ta muốn phát hiện — thì nó sẽ biến thành mục checklist không đạt hoặc lỗi nặng, và **trừ điểm lần Đầy đủ** ở Bước 5, trong khi Template và Không dùng gì không chịu khoản trừ nào.

Không gỡ khoản này thì bài test chống lại GoFind hai lần: một lần cố ý qua thứ tự chạy ở Bước 3, một lần vô tình qua đây. Lần cố ý là có chủ đích và giữ nguyên; lần vô tình phải gỡ.

Rà lại kết quả chấm của lần Đầy đủ, đánh dấu mọi mục không đạt và mọi lỗi nặng **truy được về lỗi cài cố ý**, rồi tính lại:

| | Mục đạt / tổng | Lỗi nặng | Điểm sản phẩm |
|---|---|---|---|
| Đầy đủ — như đã chấm | | | |
| Đầy đủ — trừ thiệt hại lỗi cài | | | |

**Báo cáo cả hai số.** Số dưới dùng để so với Template và Không dùng gì ở Bước 9, vì chỉ số đó mới cùng điều kiện. Số trên giữ lại vì nó trả lời một câu khác: nếu một yêu cầu sai lọt vào tài liệu thật, thiệt hại tới sản phẩm cuối là bao nhiêu.

Lỗi cài không tới được code thì hai dòng bằng nhau — ghi rõ như vậy, đừng bỏ trống.

---

## Bước 8 — Lọc trước khi kết luận

| Kiểm tra | Nếu đúng thì làm gì |
|---|---|
| Thứ hạng điểm sản phẩm trùng hoàn toàn thứ hạng số từ | Tự đọc lại kết quả hạng nhất và hạng ba bằng mắt, xem có đồng ý không |
| Chênh lệch điểm mỗi giờ giữa hai cách dưới 15% | Ghi "không phân biệt được", không dùng để kết luận |
| Một cách không hoàn thành trong 4 giờ | Vẫn tính, phần chưa làm là không đạt |
| Có lỗi phát hiện bằng đọc code mà không tái hiện được | Bỏ khỏi số lỗi |
| Chưa chạy lần thứ hai | Nhóm 3 ghi "chưa đo". Kết luận chỉ là dấu hiệu ban đầu |

---

## Bước 9 — Tính và đọc kết quả

| | Điểm sản phẩm | Giờ làm | Giờ sửa thêm | Tổng giờ | **Điểm mỗi giờ** |
|---|---|---|---|---|---|
| Đầy đủ | | | | | |
| Template | | | | | |
| Không dùng gì | | | | | |

Dòng Đầy đủ lấy **điểm đã trừ thiệt hại lỗi cài** ở Bước 7 — đó là số duy nhất cùng điều kiện với hai dòng kia. Ô nào đến từ `>4` thì đánh dấu `*`.

Ví dụ đọc bảng:

| | Điểm | Giờ làm | Giờ sửa | Tổng giờ | Điểm mỗi giờ |
|---|---|---|---|---|---|
| Đầy đủ | 85 | 4 | 1 | 5 | 17 |
| Template | 70 | 3 | 1 | 4 | 17,5 |
| Không dùng gì | 60 | 2,5 | 2 | 4,5 | 13,3 |

Đọc: GoFind hơn hẳn việc không dùng gì (17 so 13,3), nhưng **không hơn** việc chỉ dùng template (17 so 17,5). Kết luận: giá trị nằm ở mấy cái template, không ở agent và workflow.

Áp bảng kết luận ở mục 5 của `1-Criteria.md`, rồi tick 5 dấu hiệu ở mục 7.

---

## Bước 10 — Nhóm 3 cần thêm một lần chạy

Nhóm 3 (tính đều) là lời hứa cốt lõi của một framework, và một lần chạy không đo được. Bước này không phải tuỳ chọn: "đều hơn" là một trong hai mục tiêu đã chọn ở §1 của `1-Criteria.md`. Bỏ bước này thì kết luận chỉ đúng một nửa, và phải ghi rõ "Nhóm 3: chưa đo" cạnh kết luận.

Cách rẻ nhất: chạy lại **chỉ cách Đầy đủ**, trên **đúng đề bài cũ**. Không cần chạy lại hai cách kia.

**Cùng đề, không phải đề khác.** Hai lần cùng đề đo *độ ổn định giữa các lần chạy* — đúng thứ Nhóm 3 hỏi. Hai đề khác nhau đo *khả năng khái quát*, một câu hỏi khác, và kết quả còn lẫn chênh lệch độ khó giữa hai đề nên không tách được nguyên nhân.

Cái giá phải trả: lần hai đã quen bài nên thường cao điểm hơn. Vì vậy **chênh lệch chỉ đọc được theo một chiều** — lần hai thấp hơn hoặc lệch nhiều thì kết luận "chưa đều" là chắc; lần hai cao hơn thì chưa nói lên điều gì, vì không tách được phần do quen bài. Nghỉ ít nhất 1 tuần giữa hai lần để giảm bớt.

| | Điểm sản phẩm |
|---|---|
| Đầy đủ, lần 1 | |
| Đầy đủ, lần 2 (cùng đề) | |
| **Chênh lệch tuyệt đối** | |

Chênh lệch **trên 15 điểm** → ghi "chưa đều" và áp dòng bổ sung ở §5 của `1-Criteria.md`. Dưới 15 điểm → framework tạo ra kết quả lặp lại được, trong phạm vi một người và một đề bài.

---

## Bước 11 — Kiểm tra riêng phần cài đặt

Phần này không liên quan tới hiệu quả của phương pháp, nhưng là lỗi mất dữ liệu nên phải kiểm.

| Kiểm tra | Cách làm | Đạt/Không |
|---|---|---|
| Update có giữ file trong `_gf/custom/` | Tạo file, chạy `install --action quick-update`, xem còn không | |
| Uninstall có giữ `_gf-output/` | Tạo file, chạy `uninstall --yes`, xem còn không | |
| Chạy `quick-update` hai lần có ra kết quả giống nhau | so `_gf/` giữa hai lần | |
| Update có giữ `_gf/_config/` | Sửa file, chạy update, xem còn không | |

Hai dòng đầu không đạt là lỗi mất dữ liệu người dùng — sửa trước khi phát hành, độc lập với kết quả hiệu quả.