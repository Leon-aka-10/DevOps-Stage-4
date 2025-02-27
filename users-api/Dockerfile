# Stage 1: Build the Java Spring Boot Users API
FROM maven:3.8.6-openjdk-11 AS builder

# Set working directory
WORKDIR /app

# Copy Maven configuration and dependencies
COPY pom.xml .  
RUN mvn dependency:go-offline

# Copy application source code
COPY src ./src  

# Build the application, skipping tests for faster builds
RUN mvn package -DskipTests

# Stage 2: Run the application using OpenJDK JRE
FROM openjdk:8-jre-slim

# Set working directory
WORKDIR /app

# Copy the built JAR file from the builder stage
COPY --from=builder /app/target/*.jar ./users-api.jar

# Set environment variables from .env
ENV SERVER_PORT=${SERVER_PORT}
ENV JWT_SECRET=${JWT_SECRET}

# Expose the application port dynamically
EXPOSE ${SERVER_PORT}

# Run the Spring Boot application
CMD ["java", "-jar", "users-api.jar"]