# ADR-001: Lựa chọn nền tảng LLM Gateway cho team

| | |
|---|---|
| **Trạng thái** | Proposed |
| **Ngày** | 2026-07-22 |
| **Người đề xuất** | Kaii |
| **Phạm vi** | Gateway gọi LLM dùng chung cho team (bắt đầu ở máy cá nhân, tiến tới production) |

## 1. Bối cảnh

Team cần một lớp trung gian (gateway) để gọi nhiều LLM provider (OpenAI, Anthropic, và có thể thêm Bedrock/Vertex/Azure sau) qua một interface thống nhất, với các yêu cầu:

- Virtual API key riêng theo từng thành viên/team, có budget và rate limit
- Theo dõi chi phí (spend tracking) theo key/user/team
- Kiểm soát bảo mật: dữ liệu request/response không rời khỏi hạ tầng mình kiểm soát
- Authentication cho nhiều người dùng (tối thiểu là key riêng; có thể cần SSO khi team lớn hơn)
- Chi phí vận hành phù hợp với quy mô một team nhỏ, chưa cần scale doanh nghiệp

Có 3 hướng để đáp ứng nhu cầu này:

1. **Tự xây dựng (Build in-house)** — viết một lớp abstraction gọi LLM API của riêng team
2. **LiteLLM (self-hosted)** — dùng thư viện/gateway mã nguồn mở, tự host
3. **Portkey (SaaS trả phí)** — nền tảng AI Gateway thương mại đang phổ biến trên thị trường, có bản SaaS quản lý sẵn

## 2. Các yếu tố ra quyết định

- **Chi phí**: một lần (setup) + vận hành liên tục (theo tháng/theo scale)
- **Thời gian đưa vào dùng được**: từ lúc bắt đầu đến lúc team dùng ổn định
- **Kiểm soát dữ liệu**: request/response, API key, log có rời khỏi hạ tầng của mình không
- **Độ trưởng thành tính năng**: số provider hỗ trợ, virtual keys, budget, guardrails, observability
- **Gánh nặng vận hành & bảo trì lâu dài**
- **Rủi ro phụ thuộc nhà cung cấp (vendor lock-in / vendor risk)**
- **Khả năng mở rộng** khi team lớn hơn hoặc yêu cầu compliance cao hơn

## 3. Các phương án được xem xét

### Phương án A — Tự xây dựng in-house

Viết một service nhỏ wrap các SDK của OpenAI/Anthropic/... thành một API nội bộ, tự làm key management, rate limit, logging.

**Ưu điểm**
- Toàn quyền kiểm soát kiến trúc, không phụ thuộc roadmap của bên thứ ba
- Không có chi phí license/subscription
- Dễ tuỳ biến logic đặc thù của công ty (VD: business rule riêng khi route model)

**Nhược điểm**
- Phải tự làm lại những gì cả LiteLLM và Portkey đã làm sẵn: chuẩn hoá format response giữa các provider, retry/fallback, connection pooling, mã hoá key at-rest, virtual key + budget, audit log...
- Chi phí kỹ sư không nhỏ: ước tính thô cho bản MVP (multi-provider abstraction cơ bản + virtual key + rate limit) khoảng **4–8 tuần-người** để làm được phần nền tảng, và cần duy trì liên tục (~0.25–0.5 FTE) để theo kịp thay đổi API của từng provider (model mới, breaking change, pricing mới)
- Mỗi provider ra model mới hoặc đổi API là một điểm phải tự vá — LiteLLM/Portkey đã có cộng đồng/team riêng làm việc này liên tục
- Rủi ro bảo mật cao hơn nếu team chưa có kinh nghiệm về mã hoá key, TLS, secret management — dễ bỏ sót so với một dự án đã được audit (SOC2, pentest…)
- Thời gian đưa vào dùng chậm nhất trong 3 phương án

**Chi phí ước tính**: $0 license, nhưng chi phí kỹ sư (lương + thời gian) là đáng kể và kéo dài vô hạn (maintenance).

### Phương án B — LiteLLM (self-hosted)

Self-host LiteLLM Proxy (OSS, miễn phí) trên hạ tầng của team; nâng lên Enterprise (custom pricing) chỉ khi cần SSO cho >5 người, RBAC nâng cao, hoặc SLA support.

**Ưu điểm**
- Bản OSS miễn phí, có sẵn: 100+ provider, virtual keys, budget/rate limit theo key/user/team, load balancing, fallback, guardrails cơ bản, TLS in-transit
- Tự host = dữ liệu không rời khỏi hạ tầng của team; không gửi telemetry về LiteLLM
- Đã được dùng ở scale lớn (Netflix, Lemonade — theo case study công khai), cộng đồng lớn (52K+ GitHub stars, 1000+ contributor), giảm rủi ro "dự án chết"
- Có đường nâng cấp rõ ràng lên Enterprise khi cần (SSO, audit log, JWT/OIDC mapping) mà không phải đổi nền tảng
- Vì mã nguồn mở, không bị khoá chặt vào một vendor — có thể fork nếu cần

**Nhược điểm**
- Team tự chịu trách nhiệm vận hành: patch, upgrade, monitor uptime, backup DB
- Một số điểm bảo mật mặc định chưa chặt (cache và log lưu plaintext) — cần tự cấu hình đúng (`LITELLM_SALT_KEY`, tắt log nhạy cảm, TLS) như đã thống nhất ở phần bảo mật
- Tính năng SSO cho >5 người, RBAC nâng cao, audit log chi tiết nằm ở bản Enterprise (custom pricing, phải liên hệ sales)
- Là công ty được Y Combinator hậu thuẫn — rủi ro vendor thay đổi hướng đi hoặc bị mua lại vẫn tồn tại (dù ít ảnh hưởng vì là OSS, có thể tự vận hành tiếp không cần vendor)

**Chi phí ước tính**: $0 license (OSS) + chi phí hạ tầng (server nhỏ + Postgres, có thể vài USD–vài chục USD/tháng ở quy mô team nhỏ) + thời gian setup ban đầu tính bằng giờ/ngày, không phải tuần.

### Phương án C — Portkey (SaaS trả phí, đại diện nhóm "hot" trên thị trường)

Portkey là AI Gateway thương mại phổ biến, có bản SaaS quản lý sẵn (không cần tự host), pricing công khai theo tier.

**Ưu điểm**
- Không cần tự vận hành hạ tầng gateway — Portkey quản lý toàn bộ, setup nhanh (vài phút, "3 lines of code")
- UI/observability, prompt management, guardrails, eval templates trưởng thành hơn LiteLLM ở một số mảng (theo mô tả sản phẩm)
- Có SLA, support production ở tier trả phí
- Vừa được **Palo Alto Networks hoàn tất mua lại (2026)** — có thể mang lại backing tài chính/bảo mật enterprise mạnh hơn, nhưng cũng là điểm cần theo dõi (xem Rủi ro)

**Nhược điểm**
- **Chi phí định kỳ, tăng theo scale**: Free tier chỉ 10K log/tháng, retention 3 ngày, không phù hợp production. Tier "Production" $49/tháng cho 100K log, vượt hạn mức tính phụ thu **$9/100K log**. Virtual key + budgeting, semantic caching, RBAC nằm ở tier trả phí. Enterprise (SSO, VPC hosting, HIPAA/SOC2, private tenancy) là custom pricing, phải liên hệ sales
- Bản self-host/open-source của Portkey bị cắt giảm nhiều tính năng (không có virtual key có budget, không semantic caching, dashboard cơ bản) — muốn đầy đủ tính năng bắt buộc phải dùng SaaS trả phí hoặc Enterprise
- Theo mặc định, request đi qua hạ tầng SaaS của Portkey (bên thứ ba) — muốn giữ dữ liệu trong VPC riêng phải mua Enterprise
- **Rủi ro hậu sáp nhập**: việc bị Palo Alto Networks mua lại tạo ra sự không chắc chắn về roadmap, giá, điều khoản hợp đồng trong ngắn-trung hạn — cần theo dõi trước khi ký hợp đồng dài hạn
- Thêm một chi phí SaaS định kỳ nữa vào ngân sách, tăng dần theo lượng request — không lý tưởng cho team nhỏ, nhạy cảm chi phí

**Chi phí ước tính**: $0 (free, không đủ cho production) → $49+/tháng (Production, tăng theo overage) → custom (Enterprise). Không có chi phí hạ tầng tự vận hành, nhưng đổi lại là subscription liên tục.

## 4. So sánh nhanh

| Tiêu chí | Tự build | LiteLLM (self-host) | Portkey (SaaS) |
|---|---|---|---|
| Chi phí license | $0 | $0 (OSS) / custom (Enterprise) | $0 → $49+/tháng → custom |
| Chi phí thực tế chính | Kỹ sư (lương, dài hạn) | Hạ tầng nhỏ + setup | Subscription tăng theo scale |
| Thời gian đưa vào dùng | Chậm (tuần) | Nhanh (giờ–ngày) | Nhanh nhất (phút) |
| Dữ liệu ở đâu | Hạ tầng mình | Hạ tầng mình | Bên thứ ba (trừ khi mua Enterprise VPC) |
| Số provider hỗ trợ sẵn | 0 (tự viết) | 100+ | Nhiều, tương đương |
| Virtual key + budget | Tự làm | Có sẵn (OSS) | Chỉ từ tier trả phí |
| SSO / RBAC nâng cao | Tự làm | Enterprise (custom) | Production/Enterprise (trả phí) |
| Vận hành/bảo trì | Toàn bộ do team | Do team (nhưng nhẹ hơn build) | Do vendor |
| Rủi ro vendor | Không có (tự chủ) | Thấp (OSS, có thể fork) | Trung bình (đang hậu sáp nhập) |

## 5. Quyết định

Chọn **Phương án B — LiteLLM self-hosted (bản OSS)**, bắt đầu triển khai ở máy cá nhân trước khi đưa lên môi trường dùng chung cho team.

**Lý do chính:**
- Đáp ứng đủ yêu cầu hiện tại (virtual key, budget, spend tracking, đa provider) mà không tốn chi phí license
- Giữ dữ liệu hoàn toàn trong hạ tầng tự kiểm soát — quan trọng vì team đã xác định ưu tiên bảo mật dữ liệu
- Thời gian triển khai tính bằng giờ, không phải tuần như tự build
- Không tạo áp lực chi phí định kỳ tăng theo scale như Portkey — phù hợp với quy mô team nhỏ hiện tại
- Vẫn có đường nâng cấp lên Enterprise nếu sau này cần SSO/RBAC nâng cao, không phải đổi nền tảng từ đầu

Không chọn tự build vì chi phí kỹ sư và bảo trì dài hạn không tương xứng với lợi ích (đang cố gắng đi nhanh, không cần tuỳ biến sâu). Không chọn Portkey ở giai đoạn này vì chi phí subscription tăng theo scale không cần thiết khi LiteLLM OSS đã đáp ứng đủ, và có thêm yếu tố rủi ro cần quan sát từ vụ sáp nhập Palo Alto Networks.

## 6. Hệ quả

**Tích cực**
- Không phát sinh chi phí license ở giai đoạn hiện tại
- Toàn quyền kiểm soát dữ liệu và hạ tầng
- Có thể mở rộng dần (thêm provider, thêm Postgres cho virtual keys) không cần kiến trúc lại

**Cần đánh đổi / theo dõi**
- Team phải tự vận hành: theo dõi uptime, backup DB, cập nhật phiên bản LiteLLM
- Phải tự cấu hình đúng các điểm bảo mật mặc định chưa chặt (salt key, tắt log nhạy cảm, TLS) — nếu bỏ sót sẽ là lỗ hổng
- Khi team vượt quá ~5 người cần SSO thật, hoặc cần compliance cao (SOC2/HIPAA khách hàng yêu cầu), sẽ cần đánh giá lại: nâng lên LiteLLM Enterprise hay chuyển hướng khác

## 7. Điều kiện xem lại quyết định (Revisit triggers)

Xem lại ADR này nếu xảy ra một trong các điều kiện sau:

- Team vượt 5 người cần SSO chính thức, hoặc cần RBAC/audit log nâng cao thường xuyên
- Khách hàng/đối tác yêu cầu chứng nhận compliance (SOC2 Type II, HIPAA...) mà bản OSS không đáp ứng
- Gánh nặng vận hành tự host (uptime, upgrade, bảo mật) vượt quá khả năng của team, cần chuyển sang giải pháp managed
- Chi phí hạ tầng self-host (server, DB, người vận hành) vượt chi phí subscription của giải pháp SaaS tương đương ở cùng mức tính năng

## 8. Tài liệu tham khảo

- LiteLLM Docs — Getting Started: https://docs.litellm.ai/docs/
- LiteLLM Pricing: https://www.litellm.ai/#pricing
- LiteLLM Data Privacy & Security: https://docs.litellm.ai/docs/data_security
- LiteLLM Self-Hosted Security & Encryption FAQ: https://docs.litellm.ai/docs/proxy/security_encryption_faq
- LiteLLM Virtual Keys / Authentication: https://docs.litellm.ai/docs/proxy/virtual_keys
- Portkey Pricing: https://portkey.ai/pricing
- Thông báo Palo Alto Networks hoàn tất mua lại Portkey (2026): https://www.paloaltonetworks.com/company/press/2026/palo-alto-networks-to-acquire-portkey-to-secure-the-rise-of-ai-agents