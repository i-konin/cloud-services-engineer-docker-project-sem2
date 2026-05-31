FROM golang:1.24-alpine AS builder
WORKDIR /app
RUN apk add --no-cache git
COPY go.mod go.sum ./
RUN go mod download
COPY . .
RUN CGO_ENABLED=0 GOOS=linux go build -o main ./cmd/api

FROM alpine:latest
RUN addgroup -S appgroup && adduser -S appuser -G appgroup
WORKDIR /app
RUN mkdir -p /app/logs && chown -R appuser:appgroup /app/logs
COPY --from=builder /app/main /usr/local/bin/main
RUN chmod +x /usr/local/bin/main && chown appuser:appgroup /usr/local/bin/main
USER appuser
EXPOSE 8081
CMD ["main"]