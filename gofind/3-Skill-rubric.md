# RUBRIC TỰ REVIEW

**Luồng chính Go Find — 5 bước**

*plan-from-issue → review-plan → finalize-plan → dev-from-plan → code-review*

Bộ tiêu chí đạt/không đạt + metric + tình huống test, bám đúng hợp đồng hành vi của từng skill

Ngày lập: 24/07/2026 · Người soạn: Kaii

> Tài liệu này chấm **từng skill** có làm đúng hợp đồng hành vi của nó không — góc nhìn hộp trắng.
> `1-Criteria.md` + `2-How-to-test.md` chấm **cả framework** có hiệu quả hơn baseline không — góc nhìn hộp đen.
> Hai câu hỏi khác nhau, và câu trả lời có thể ngược chiều: năm skill đều đạt mà framework vẫn không hiệu quả hơn chat thường, hoặc ngược lại. Chạy rubric này trước, vì nó chỉ ra hỏng ở đâu; bộ kia chỉ nói hiệu quả tổng thể bao nhiêu.

# 1. Cách dùng rubric này

Mỗi skill được chấm theo hai loại tiêu chí, tách bạch để tránh nhập nhằng:

- **Kiểm bắt buộc (Pass/Fail):** các hành vi trong "hợp đồng" của skill — hoặc làm đúng, hoặc trượt. Bất kỳ mục Pass/Fail nào FAIL thì skill tự động xếp "Chưa đạt", bất kể điểm chất lượng.
- **Chấm chất lượng (1–5):** mức độ làm tốt phần còn lại (độ đầy đủ, rõ ràng, hữu ích). 1 = kém, 3 = dùng được, 5 = xuất sắc.

Mọi hạng mục trong tài liệu này đều mang đúng **một** nhãn: `[PF]` hoặc `(1–5)`. Không có mục nào vừa là cổng cứng vừa là điểm mềm.

**Mục 1–5 có gắn ngưỡng thì quy về điểm như sau.** Vài hạng mục chất lượng có kèm một con số (Coverage ≥ 0.90, Accuracy ≥ 0.85, Recall ≥ 0.90…). Ngưỡng đó là **mốc của điểm 4**, không phải ranh giới đạt/trượt:

| Kết quả đo so với ngưỡng | Điểm |
|---|:---:|
| Vượt ngưỡng từ 0.05 trở lên | 5 |
| Đạt ngưỡng, tới dưới ngưỡng 0.05 | 4 |
| Dưới ngưỡng 0.05 – 0.10 | 3 |
| Dưới ngưỡng 0.10 – 0.20 | 2 |
| Dưới ngưỡng trên 0.20 | 1 |

Ví dụ: ngưỡng Recall 0.90, đo được 0.88 → điểm 4. Đo được 0.72 → điểm 2. Không có ngưỡng nào biến một mục 1–5 thành trượt — chỉ `[PF]` mới làm được điều đó.

Cách chạy để con số đáng tin:

- **Có ground truth:** chấm trên bộ tình huống test đã biết trước đáp án (Mục 6) — ví dụ đã cố tình cài đường dẫn giả, lỗi giả, ca cần sửa contract.
- **Chạy 5 lần/tình huống:** đo cả tính ổn định, không kết luận từ một lần chạy. Năm lần, không phải 3–5, để khớp với metric Consistency (CV) ở Mục 5.
- **Soi artifact, không soi cảm giác:** mỗi bước đều có artifact cụ thể (overview, reuse YAML, CR, final plan, diff, bản review) — chấm trên chính artifact đó.
# 2. Sáu tiêu chí chung cho mọi skill

Áp cho cả 5 skill, trước khi vào tiêu chí riêng.

| **Tiêu chí** | **Ý nghĩa** | **Loại** |
| --- | --- | --- |
| Tuân hợp đồng (Contract adherence) | Làm đúng việc được giao và KHÔNG vượt ranh giới (vd plan-from-issue không được code) | Pass/Fail |
| Bám thực tế (Grounding) | Mọi đường dẫn/tham chiếu có thật trong repo; không bịa file, quan hệ, dữ liệu | Pass/Fail |
| An toàn cổng (Gate/HALT) | Dừng đúng chỗ: contract gate, review gate, tiền điều kiện — không tự phong trạng thái | Pass/Fail |
| Đầy đủ đầu ra | Artifact có đủ mục bắt buộc theo định dạng của skill | 1–5 |
| Ổn định (Consistency) | Chạy lại cho kết quả tương đương | 1–5 |
| Khả dụng artifact | Con người đọc/hành động được ngay, cấu trúc rõ | 1–5 |

Ba mục **Pass/Fail** ở trên là nguyên tắc, không phải hạng mục chấm riêng — chúng đã được cụ thể hoá thành các mục `[PF]` của từng skill ở Mục 3. Chấm ở đó, đừng chấm hai lần.

Ba mục **1–5** ở trên thì ngược lại: chấm riêng, và chấm cho **cả 5 skill**. Chúng cộng vào điểm chất lượng cùng với các mục 1–5 của từng skill (xem công thức ở Mục 7).

# 3. Rubric theo từng skill

## ①  gf-plan-from-issue  →  overview (đề xuất)

| **Hạng mục kiểm** | **Cách kiểm** | **Đạt khi** |
| --- | --- | --- |
| [PF] Không code, không viết final plan | Xem đầu ra chỉ là overview | Chỉ sinh overview; không đụng code |
| [PF] Đường dẫn tham chiếu có thật | Đối chiếu path trong overview với repo | 100% path tồn tại |
| [PF] Hỏi làm rõ khi mơ hồ | Cho issue thiếu thông tin | Đặt câu hỏi, không đoán bừa |
| [PF] Sub-issue → MỘT overview + dependency map | Chạy issue cha có con | Gộp 1 file, xếp topo đúng |
| (1–5) Nắm đúng ý đồ & AC issue | So overview với nội dung issue | Ngưỡng điểm 4: Coverage 0.90 |
| (1–5) Có mục Proposed Components/Functions | Kiểm cấu trúc | Liệt kê đủ, cụ thể |
| (1–5) Design & User Flow (nếu có Figma) | Kiểm mục design; ca không Figma | Có khi có / bỏ khi không, không bịa |

## ②  gf-review-plan  →  verdict + reuse-analysis.yaml + CR

**Skill quan trọng nhất — nó là cổng chất lượng.** Chấm nghiêm nhất ở đây.

| **Hạng mục kiểm** | **Cách kiểm** | **Đạt khi** |
| --- | --- | --- |
| [PF] Bắt đường dẫn giả | Cài sẵn path không tồn tại trong overview | Phát hiện 100% path giả |
| [PF] Cổng contract | Cài ca buộc sửa openapi.yaml/ERD.md | HALT + tạo CR + trả NOT READY |
| [PF] Không âm thầm đổi contract | Kiểm không tự sửa contract | Luôn qua CR để người duyệt |
| [PF] Sub-issue dependency coverage | Cài con bị bỏ / xếp sai / vòng lặp | Báo BLOCKER đúng |
| (1–5) Phân loại reused/extend/new (Semble) | So với đáp án phân loại đã biết | Ngưỡng điểm 4: Accuracy 0.85 |
| (1–5) Tách phần dùng chung (build once) | Cài 2 component trùng phần chung | Nhận ra & gộp |
| (1–5) Đo impact cho item extend (GitNexus) | Kiểm item extend có blast-radius | Có impact, hợp lý |
| (1–5) Verdict đúng thực tế | So READY/NOT READY với đáp án | Ngưỡng điểm 4: Recall lỗi 0.90 |

## ③  gf-finalize-plan  →  final plan (draft → ready-for-dev)

| **Hạng mục kiểm** | **Cách kiểm** | **Đạt khi** |
| --- | --- | --- |
| [PF] Không đóng ready-for-dev nếu review chưa PASSED | Cài ca review trả NOT READY | Giữ ở draft, không tự phong |
| [PF] CR chưa duyệt / hết vòng → HALT ở draft | Cài CR treo hoặc quá max_iterations | Dừng, không trôi xuống dev |
| (1–5) Gộp trung thực overview+review+YAML+CR | Đối chiếu final plan với nguồn | Không rơi mục, không thêm bịa |
| (1–5) Reuse Map đúng (reused/extend/new) | So quyết định với reuse YAML | Mang đúng phân loại |
| (1–5) Vòng lặp có cải tiến thật | Xem input đổi giữa các vòng | Mỗi vòng có revision, không lặp rỗng |
| (1–5) Mang Design & User Flow + Sub-issue Map | Kiểm final plan | Đủ khi nguồn có |

## ④  gf-dev-from-plan  →  thay đổi code theo AC

| **Hạng mục kiểm** | **Cách kiểm** | **Đạt khi** |
| --- | --- | --- |
| [PF] Chỉ chạy trên plan ready-for-dev | Đưa plan còn draft | Từ chối / cảnh báo, không chạy bừa |
| [PF] Không build ngoài scope plan | Cài design có element ngoài plan | Surface plan gap, route back finalize |
| (1–5) Thỏa AC của plan | Đếm AC được đáp ứng & kiểm chứng | Ngưỡng điểm 4: AC Met 0.95 |
| (1–5) Áp code_standards đúng | Kiểm coding-standards (mọi task) + frontend-patterns (UI) | Áp đúng, bỏ FE khi backend-only |
| (1–5) Triển khai theo thứ tự phụ thuộc | Kiểm thứ tự task | UI sau backend nó phụ thuộc |
| (1–5) Chạy kiểm tra repo & báo cáo | Xem có chạy test/checks | Ngưỡng điểm 4: pass@1 0.85; có report |

## ⑤  gf-code-review  →  bản review cho diff

| **Hạng mục kiểm** | **Cách kiểm** | **Đạt khi** |
| --- | --- | --- |
| [PF] Review trên diff local trước merge | Kiểm phạm vi review | Đúng phần diff, trước merge |
| [PF] Không tự sửa code khi đang review | Xem có commit/edit nào phát sinh không | Chỉ báo cáo, người quyết định sửa |
| [PF] Không bỏ sót file trong diff | Đối chiếu danh sách file review với `git diff --name-only` | Phủ 100% file thay đổi |
| (1–5) Bắt đúng lỗi thật | Cài bộ lỗi gài sẵn (seeded bugs) | Ngưỡng điểm 4: Recall 0.90 |
| (1–5) Ít báo động giả | Đếm cảnh báo sai | Ngưỡng điểm 4: Precision 0.80 |
| (1–5) Phân loại mức độ (triage) đúng | So critical/minor với đáp án | Ngưỡng điểm 4: Severity Accuracy 0.85 |
| (1–5) Góp ý hành động được | Kiểm có vị trí + cách sửa | Nêu đúng file:dòng + cách sửa cụ thể |
| (1–5) Chiếu chuẩn gf-coding-standards | Xem có đối chiếu chuẩn | Có tham chiếu chuẩn |

Hai mục `[PF]` giữa là bổ sung: trước đó ⑤ chỉ có một cổng cứng trong khi ① có bốn, khiến lưới chắn cuối cùng trước merge gần như không có quyền phủ quyết.

# 4. Kiểm "mối nối" giữa các bước

Nhiều lỗi không nằm trong một skill mà ở chỗ chuyển giao. Đây là phần dễ bị bỏ sót khi chỉ review từng skill riêng lẻ.

| **Mối nối** | **Cần kiểm** |
| --- | --- |
| overview → review | Review đọc đúng overview; không bỏ qua component nào trong overview |
| review → finalize | Final plan phản ánh đúng verdict + reuse YAML + CR; không finalize khi còn CR treo |
| finalize gate | ready-for-dev CHỈ xuất hiện sau review_status: PASSED; NOT READY → tự revise ≤ max_iterations rồi HALT |
| finalize → dev | Dev chỉ nhận plan ready-for-dev; đọc đúng Reuse Map (không dựng lại phần reused) |
| dev → code-review | Review chạy trên đúng diff mà dev tạo; đối chiếu lại AC của plan |
| Nhánh Figma | Screen chỉ gắn task UI; user flow trace mọi task (kể cả backend); mối nối UI↔backend có task đỡ |

**Nguyên tắc sửa lỗi cần verify:** "muốn output khác thì input phải khác". Kiểm rằng chạy lại một skill mà không kèm {revision_note} thì KHÔNG nên kỳ vọng artifact đổi — và sửa ở đúng tầng sở hữu lỗi, không vá tầng dưới.

# 5. Bộ metric tổng hợp nên ghi lại

| **Metric** | **Đo cái gì** | **Ngưỡng gợi ý** |
| --- | --- | --- |
| Contract-adherence rate | % lần skill không vượt ranh giới của nó | 100% |
| Path-hallucination catch | gf-review-plan bắt được path giả | 100% |
| Gate integrity | % lần cổng (contract/review/precondition) hoạt động đúng | 100% |
| Reuse classification accuracy | Đúng reused/extend/new | ≥ 0.85 |
| Verdict Recall / Precision | gf-review-plan bắt lỗi / ít báo giả | R ≥ 0.90, P ≥ 0.80 |
| Code-review Recall / Precision | Trên seeded bugs | R ≥ 0.90, P ≥ 0.80 |
| AC Met (dev) | % AC được thỏa & kiểm chứng | ≥ 0.95 |
| Test pass@1 | Code chạy đúng ngay | ≥ 0.85 |
| Consistency (CV) | Độ lệch qua 5 lần chạy | ≤ 0.10 |
| Rework rounds | Số vòng phải sửa để đạt | ≤ 1 |

# 6. Bộ tình huống test gợi ý (golden scenarios)

Để chấm được các mục Pass/Fail và metric ở trên, cần cố tình dựng sẵn các ca có đáp án:

- **Đường dẫn giả:** overview có vài path/file không tồn tại → kỳ vọng gf-review-plan bắt hết.
- **Ca reuse đã biết:** issue mà một phần rõ ràng nên reused, một phần extend, một phần new → so với phân loại của Semble/GitNexus.
- **Hai component trùng phần chung:** kỳ vọng review tách shared part để build một lần.
- **Ca buộc sửa contract:** issue đòi đổi openapi.yaml/ERD.md → kỳ vọng HALT + CR + NOT READY, tuyệt đối không tự sửa.
- **Issue cha + sub-issues:** gồm ca bỏ sót con, ca xếp sai thứ tự phụ thuộc, ca có vòng lặp phụ thuộc → kỳ vọng BLOCKER đúng.
- **Plan còn draft đưa cho dev:** kỳ vọng gf-dev-from-plan từ chối / cảnh báo.
- **Design ngoài scope:** figma có state/variant plan không cover → kỳ vọng surface plan gap, route back finalize.
- **Bộ lỗi gài sẵn trong diff:** cài bug đã biết + severity đã biết → chấm Recall/Precision/severity của gf-code-review.
- **Ca song ngữ Việt–Anh:** nhân đôi vài tình huống ở cả 2 ngôn ngữ.

# 7. Thang điểm & kết luận cho mỗi skill

Điểm chất lượng (các mục 1–5) quy về 100. Nhưng Pass/Fail có quyền phủ quyết.

**Công thức:**

```
điểm chất lượng = (tổng điểm các mục 1–5) ÷ (số mục 1–5 × 5) × 100
```

Số mục 1–5 của mỗi skill = các mục `(1–5)` riêng ở Mục 3, **cộng 3 mục chất lượng chung** ở Mục 2 (đầy đủ đầu ra, ổn định, khả dụng artifact):

| Skill | Mục riêng | + chung | Tổng mục | Điểm tối đa thô |
|---|:---:|:---:|:---:|:---:|
| ① gf-plan-from-issue | 3 | 3 | 6 | 30 |
| ② gf-review-plan | 4 | 3 | 7 | 35 |
| ③ gf-finalize-plan | 4 | 3 | 7 | 35 |
| ④ gf-dev-from-plan | 4 | 3 | 7 | 35 |
| ⑤ gf-code-review | 5 | 3 | 8 | 40 |

Chia cho điểm tối đa thô rồi nhân 100 nên năm skill so được với nhau, dù số hạng mục khác nhau. Ví dụ ② được 28/35 → 80/100.

Mọi mục 1–5 có trọng số bằng nhau. Muốn đổi thì đổi công thức chứ đừng chấm lệch tay — cách sau không ai kiểm lại được.

- **Đạt (Sẵn sàng dùng):** mọi mục [PF] PASS và điểm chất lượng ≥ 80/100.
- **Cần giám sát:** mọi [PF] PASS nhưng chất lượng 65–79 → dùng được nhưng người phải soi artifact.
- **Chưa đạt:** bất kỳ mục [PF] nào FAIL, hoặc chất lượng < 65 → cải tiến skill/prompt rồi test lại.

**Vì sao Pass/Fail có quyền phủ quyết:** với luồng này, một cổng hỏng (vd tự phong ready-for-dev, hay âm thầm đổi contract) nguy hiểm hơn nhiều so với một artifact hơi sơ sài — nên nó phải là điều kiện cứng, không bù trừ bằng điểm đẹp ở chỗ khác.

*Tóm tắt: chấm mỗi skill trên các mục Pass/Fail (hợp đồng, grounding, cổng) + điểm chất lượng 1–5, dựa trên golden scenarios có cài sẵn đáp án (path giả, reuse, contract-change, sub-issue deps, seeded bug), chạy nhiều lần; đừng quên kiểm mối nối giữa các bước và cổng finalize.*
