# Phân tích lỗi và khắc phục sự cố `.gitlab-ci.yml` (User Service)

## 1. Các lỗi và thiếu sót trong cấu hình cũ
1. **Sai cú pháp khai báo `stages`:**
   - Cấu hình cũ viết `stages: build_app` (dạng chuỗi), không đúng chuẩn danh sách của GitLab CI.
   - Cần sửa thành dạng mảng:
     ```yaml
     stages:
       - build_app
     ```
2. **Thiếu `image` môi trường JDK 17 / Gradle:**
   - File cũ không khai báo `image`, khiến job chạy trên môi trường mặc định không đảm bảo có sẵn Java 17 và môi trường build Gradle cho ứng dụng Spring Boot.
   - Bổ sung `image: gradle:8.11.1-jdk17` (hoặc `eclipse-temurin:17-jdk`) để cung cấp đầy đủ JDK 17 và Gradle.
3. **Đảm bảo quyền thực thi cho Gradle Wrapper (`./gradlew`):**
   - Thêm `chmod +x gradlew` trước khi chạy `./gradlew clean build -x test` để tránh lỗi thiếu quyền thực thi trên môi trường Linux của runner.

## 2. Cách kiểm tra kết quả
- Kiểm tra cú pháp YAML hợp lệ.
- Khi chạy job `build_job` ở stage `build_app`, lệnh `./gradlew clean build -x test` thực thi thành công và tạo ra artifact build mà không chạy test.