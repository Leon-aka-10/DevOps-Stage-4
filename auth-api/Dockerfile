# Stage 1: Build the Go application
FROM golang:1.19 AS build
WORKDIR /app
COPY go.mod go.sum ./
RUN go mod tidy
COPY . .
RUN go build -o auth-api

# Stage 2: Run the application using Alpine Linux
FROM alpine:latest
WORKDIR /root/
COPY --from=build /app/auth-api .
CMD ["./auth-api"]
EXPOSE 8080