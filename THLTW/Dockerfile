# Stage 1: Build stage
FROM maven:3.9.6-eclipse-temurin-21-alpine AS build
WORKDIR /app
COPY pom.xml .
COPY src ./src
RUN mvn clean package -DskipTests

# Stage 2: Run stage - Chuyển sang bản JRE đầy đủ (không phải alpine) để ổn định hơn
FROM eclipse-temurin:21-jre
WORKDIR /app
COPY --from=build /app/target/*.jar app.jar

# Giảm Xmx xuống một chút để dành RAM cho OS của Render
# Thêm các tham số tối ưu cho môi trường container RAM thấp
ENTRYPOINT ["java", \
            "-Xmx256m", \
            "-Xms256m", \
            "-XX:+UseSerialGC", \
            "-Xss512k", \
            "-jar", "app.jar", \
            "--server.port=${PORT:10000}"]
