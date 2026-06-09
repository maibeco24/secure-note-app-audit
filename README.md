SecureNotes - Ứng dụng Quản lý Ghi chú và Quy trình Kiểm thử Bảo mật Toàn diện

Tổng quan dự án
SecureNotes là một dự án cá nhân bao gồm việc phát triển một ứng dụng Web API phục vụ nhu cầu lưu trữ ghi chú cá nhân, kết hợp với quy trình kiểm thử xâm nhập (Penetration Testing) nghiêm ngặt theo tiêu chuẩn OWASP Top 10.

Mục tiêu chính của dự án là áp dụng tư duy DevSecOps (Bảo mật từ giai đoạn phát triển), sử dụng các công cụ chuyên dụng để tìm kiếm lỗ hổng cấu hình, lỗ hổng mã nguồn và tiến hành vá lỗi trực tiếp ở tầng ứng dụng.

Công nghệ và Công cụ sử dụng

Backend: Node.js / Express (hoặc ngôn ngữ bạn chọn).

Security Intercepting Proxy: OWASP ZAP (2.17.0).

Network Analyzer: Wireshark.

Browser Environment: Microsoft Edge và FoxyProxy Standard.

Quy trình Triển khai Bảo mật (Security Lifecycle)

Bước 1: Giám sát lưu lượng và Giải mã HTTPS

Cấu hình OWASP ZAP làm proxy trung gian để bắt toàn bộ lưu lượng HTTP/HTTPS từ trình duyệt phát ra trong quá trình sử dụng ứng dụng.

Sinh mã và import thành công ZAP Root CA Certificate vào Trình quản lý chứng chỉ hệ thống (Windows Certificate Manager) để cho phép ZAP giải mã, phân tích sâu (Deep Packet Inspection) dữ liệu bên trong các luồng SSL/TLS mà không làm gãy kết nối trình duyệt.

Bước 2: Đánh giá lỗ hổng Thụ động và Chủ động (Passive và Active Scan)

Passive Scan: Sử dụng ZAP để duyệt thụ động qua các API Endpoints công khai của ứng dụng, rà soát các thiếu sót về Security Headers và cấu hình Cookie phiên làm việc.

Active Scan: Bật tính năng tấn công chủ động, bơm các Payload độc hại để kiểm tra khả năng chống chịu của các Form nhập liệu trước lỗi chèn mã độc (Injection).

Bước 3: Phân tích tầng mạng với Wireshark

Khởi chạy Wireshark trên Card mạng mạng cục bộ, áp dụng bộ lọc hiển thị http để kiểm tra xem dữ liệu nhạy cảm (mật khẩu, token) có bị truyền đi dưới dạng văn bản thô (Cleartext) hay không.

Kết quả kiểm thử và Biện pháp vá lỗi (Remediation)

Qua quá trình chạy quét, hệ thống đã phát hiện một số điểm yếu cấu hình và đã được vá lỗi triệt để:

Lỗ hổng Cấu hình sai Bảo mật (OWASP A05:2021)

Phát hiện (ZAP): Ứng dụng ban đầu thiếu cấu hình HSTS và các cờ an toàn cho Cookie phiên làm việc (HttpOnly, Secure).

Biện pháp khắc phục (Mã nguồn): Bổ sung Middleware bảo mật để ép Header an toàn và cấu hình lại Cookie quản lý phiên:
res.cookie('session_id', token, { httpOnly: true, secure: true, sameSite: 'strict' });

Nguy cơ rò rỉ dữ liệu qua mạng (Wireshark Analysis)

Phát hiện (Wireshark): Dữ liệu truyền tải qua HTTP thuần bị Wireshark bắt trọn gói tin, để lộ thông tin cấu hình Server (Server Header Disclosure).

Biện pháp khắc phục: Cấu hình tắt tính năng tự động hiển thị phiên bản phần mềm trên Web Server và cấu hình chứng chỉ TLS để mã hóa 100% dữ liệu đường truyền.  

<img width="1220" height="837" alt="2026-06-10" src="https://github.com/user-attachments/assets/a5998063-e762-4b71-a5d4-552ffb467d90" />
<img width="1920" height="1020" alt="2026-06-10 (1)" src="https://github.com/user-attachments/assets/29fde892-ac60-4d33-a116-84e866d69d9e" />
<img width="1920" height="1020" alt="2026-06-10 (3)" src="https://github.com/user-attachments/assets/cee01800-0cc5-4dd5-ae3e-ba411637f3fe" />
<img width="1920" height="1020" alt="2026-06-10 (4)" src="https://github.com/user-attachments/assets/ba842cac-2417-4bc4-952e-71f773410528" />





