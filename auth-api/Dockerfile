# Stage 1: Build the Go application
FROM golang:1.23 AS build

# Set working directory
WORKDIR /app

# Copy dependencies and download modules
COPY go.mod go.sum ./
RUN go mod tidy && go mod download

# Copy application source code
COPY . .

# Build the application and set execution permissions
RUN go build -o auth-api && chmod +x auth-api

# Stage 2: Run the application using Alpine Linux
FROM alpine:latest

# Install required dependencies
RUN apk add --no-cache ca-certificates

# Set working directory
WORKDIR /root/

# Copy built application
COPY --from=build /app/auth-api .

# Ensure the binary is executable
RUN chmod +x /root/auth-api

# Set environment variables
ENV AUTH_API_PORT=${AUTH_API_PORT}
ENV JWT_SECRET=${JWT_SECRET}
ENV USERS_API_ADDRESS=${USERS_API_ADDRESS}

# Expose application port
EXPOSE ${AUTH_API_PORT}

# Run the application
CMD ["./auth-api"]