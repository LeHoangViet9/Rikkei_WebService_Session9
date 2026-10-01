# Sử dụng JRE siêu nhẹ làm môi trường chạy
FROM eclipse-temurin:17-jre-alpine

WORKDIR /app

# Copy file .jar đã được Pipeline sinh ra từ thư mục build
COPY build/libs/*.jar app.jar

EXPOSE 8080

ENTRYPOINT ["java", "-jar", "app.jar"]