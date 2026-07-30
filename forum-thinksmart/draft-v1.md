# Thinksmartinsurance Forum

*Overview:
- Là  nền tảng tích hợp mạng xã hội, quản lý tranning và trợ lý trí tuệ nhân tạo (AI Assistant). Hệ thống phục vụ việc kết nối cộng đồng, lưu trữ tài nguyên và tổ chức khóa học bài bản.

*Requirements:
** Functional Requirements:
- Authen - Author: Đăng ký, đăng nhập, phân quyền (Admin, Giáo viên, Học sinh).
- Communities Feed: Bảng tin cộng đồng, tương tác bài viết (like, comment, share).
- Wall of Fame: Vinh danh cá nhân xuất sắc theo bảng xếp hạng hiệu suất.
- Library: Tải lên, quản lý và phân phối tài liệu (Records, Images, Files). 
- Courses: Tạo, thiết kế bài tập và chấm điểm khóa học trực tuyến.
- AI Assistant: Trợ lý ảo hỗ trợ giải đáp thắc mắc và gợi ý.
- Admin: Quản trị người dùng, duyệt nội dung và xem báo cáo tổng quan.
** Non-functional Requirements:
- Khả năng mở rộng (Scalability): Hệ thống chịu tải tốt khi lượng truy cập bảng tin (Feed) tăng đột biến.
- Độ sẵn sàng (Availability): Cam kết uptime đạt tối thiểu 95%.
- Độ trễ (Latency): AI Assistant phản hồi nhanh và chính xác; luồng tải dữ liệu thư viện mượt mà.
- Bảo mật (Security): Mã hóa file trong thư viện; bảo vệ API chống tấn công chiếm quyền.

*High-level Architecture (Optional - Chưa cần thiết)
Client (Web/Mobile) -> API Gateway -> [Load Balancer] -> [Microservices] -> [Databases / Cache]

** Các dịch vụ cốt lõi (Core Services)
- Identity Service (Authen/Author): Quản lý phiên đăng nhập và phân quyền bằng JWT.
- Feed Service: Xử lý logic tạo bài viết, tổng hợp và phân phối bảng tin cộng đồng.
- Gamification Service (Wall of Fame): Tính toán điểm số, xếp hạng và cập nhật bảng vinh danh.
- Media & Library Service: Xử lý upload file, nén ảnh, tạo đường dẫn tải xuống an toàn.
- LMS Service (Courses): Quản lý cấu trúc khóa học, đề bài tập và trạng thái làm bài.
- AI Service: Tích hợp Gateway kết nối với các mô hình LLM (Large Language Model)

*Server, Database & Storage:
- Self-hosted: VM: (Client + Server + PostgreSQL + MinIO)
- Ưu điểm:
+ Toàn quyền kiểm soát dữ liệu và hạ tầng.
+ Dễ monitor, tối ưu hiệu năng và mở rộng.
+ Chi phí thấp hơn khi scale lớn, không bị vendor lock-in.
- Nhược điểm:
+Cần đội ngũ vận hành (DevOps).
+Tự quản lý backup, HA, bảo mật và nâng cấp.
=> Đề xuất VM: 2CPU + 4RAM + 150GB DISK (Scale up lên  4CPU + 8GB RAM)

** 3rd Party: Supabase + VM: (Client + Server)
- Ưu điểm:
+ Triển khai nhanh, ít cần vận hành.
+ Tích hợp sẵn Auth, Storage, Backup và Dashboard.
+ Phù hợp MVP hoặc dự án cần ra mắt nhanh.
- Nhược điểm:
+ Phụ thuộc nhà cung cấp (vendor lock-in).
+ Hạn chế tùy chỉnh và tối ưu hệ thống.
+ Chi phí có thể tăng khi quy mô lớn.
=> Đề xuất VM: 2CPU + 2GB RAM + 20GB DISK


