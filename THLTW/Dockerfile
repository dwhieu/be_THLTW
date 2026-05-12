# Stage 1: Build stage
FROM maven:3.9.6-eclipse-temurin-21-alpine AS build
WORKDIR /app
COPY pom.xml .
COPY src ./src
RUN mvn clean package -DskipTests

# Stage 2: Run stage
FROM eclipse-temurin:21-jre-alpine
WORKDIR /app
COPY --from=build /app/target/*.jar app.jar

# Tối ưu RAM cho gói Free của Render (giới hạn 300MB cho Java)
# Và cấu hình Port linh hoạt theo biến $PORT của Render
ENTRYPOINT ["java", "-Xmx300m", "-Xms300m", "-jar", "app.jar", "--server.port=${PORT:8080}"]
