# Kiến trúc Pipeline CI/CD cho Cart Service (Tiki-clone)

Pipeline được thiết kế theo nguyên tắc "Fail-fast" (Lỗi là dừng ngay) với 3 giai đoạn (stages) liên kết chặt chẽ:

1. **Stage: test**
   - Khởi tạo vùng chứa (container) PostgreSQL làm database tạm thời.
   - Biên dịch và chạy toàn bộ Unit/Integration Test bằng Gradle.
   - **Bẫy lỗi:** Nếu bất kỳ test case nào rớt (hoặc không kết nối được DB), pipeline lập tức báo đỏ (Failed) và hủy bỏ hoàn toàn các bước sau. Không có Artifact nào được sinh ra.

2. **Stage: build**
   - Chỉ được kích hoạt khi Stage `test` đã Passed.
   - Sử dụng lệnh `./gradlew build -x test` để đóng gói source code thành file `cart-service.jar`.
   - Lưu trữ file `.jar` này vào `artifacts` để chuyển giao cho bước cuối.

3. **Stage: dockerize**
   - Chỉ chạy trên nhánh `main`.
   - Nhận file `.jar` từ Stage `build`.
   - Tiến hành build Docker Image. Thay vì dùng tag `latest`, hệ thống tự động trích xuất biến môi trường `$CI_COMMIT_SHORT_SHA` (mã băm của lần commit) để làm tag phiên bản (VD: `cart-service:a1b2c3d`).
   - Đăng nhập bảo mật và Push image có tag này lên Docker Hub.