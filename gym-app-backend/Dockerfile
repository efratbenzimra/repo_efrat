# Stage 1: Build the application
FROM maven:3.9-amazoncorretto-17 AS builder

# Yazar bilgisi
LABEL authors="Mehmet Furkan Kaya <furkan.36kaya@gmail.com>"

# Copy the project files to the container
COPY ./pom.xml ./pom.xml

# Download all required dependencies into one layer
RUN mvn dependency:go-offline -B

# Copy your other files
COPY ./src ./src

# Build the application
RUN mvn package -DskipTests
RUN mkdir -p /app/videos

# Stage 2: Run on a lightweight Java 17 JRE image
FROM eclipse-temurin:17-jre-alpine

WORKDIR /app

COPY --from=builder --chown=10001:10001 /target/*.jar /app/app.jar
COPY --from=builder --chown=10001:10001 /app/videos /app/videos

USER 10001:10001
ENTRYPOINT ["java", "-jar", "/app/app.jar"]
