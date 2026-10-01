Phân tích: So sánh Single-stage và Multi-stage Build

Ưu điểm:

Single-stage Build (Fat Container): Cấu hình đơn giản, dễ viết. Mọi công cụ (Gradle, JDK) đều có sẵn để debug trực tiếp bên trong container nếu cần.

Multi-stage Build (Khuyên dùng): Tối ưu hóa dung lượng image cực kỳ tốt. Tăng tốc độ tải (pull) image và thời gian khởi động container. Giảm thiểu bề mặt tấn công (attack surface) do lược bỏ các công cụ build.

Nhược điểm:

Single-stage Build: Image chứa rất nhiều thành phần dư thừa không cần thiết cho lúc chạy (runtime) như mã nguồn thô, thư viện JDK, thư mục cache của Gradle.

Multi-stage Build: Cú pháp Dockerfile phức tạp hơn một chút do phải quản lý nhiều stage và sao chép (COPY) file giữa các stage.

Dung lượng dự kiến:

Single-stage Build: ~ 800MB - 1GB

Multi-stage Build: ~ 150MB - 200MB

Bảo mật mã nguồn:

Single-stage Build (Kém): Toàn bộ mã nguồn thô và cấu trúc dự án bị đóng gói trực tiếp vào image thực tế. Nếu hacker xâm nhập được container, toàn bộ mã nguồn sẽ bị lộ.

Multi-stage Build (Cao): Final image chỉ chứa duy nhất file thực thi .jar và bộ JRE siêu nhẹ. Mã nguồn gốc hoàn toàn bị loại bỏ và tiêu hủy sau khi Stage 1 kết thúc.