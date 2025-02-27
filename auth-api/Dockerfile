# Stage 1: Build the Go application
FROM golang:1.23 AS build

# Set working directory
WORKDIR /app

# Copy dependencies and download modules
COPY go.mod go.sum ./
RUN go mod tidy

# Copy application source code
COPY . .

# Build the application
RUN go build -o auth-api

# Stage 2: Run the application using Alpine Linux
FROM alpine:latest

# Set working directory
WORKDIR /root/

# Copy built application
COPY --from=build /app/auth-api .

# Set environment variables from .env
ENV AUTH_API_PORT=${AUTH_API_PORT}
ENV JWT_SECRET=${JWT_SECRET}
ENV USERS_API_ADDRESS=${USERS_API_ADDRESS}

# Expose application port
EXPOSE ${AUTH_API_PORT}

# Run the application
CMD ["./auth-api"]