FROM golang:1.22-alpine

WORKDIR /app
COPY . .
RUN go build -o main ./cmd/api

RUN addgroup -S appgroup && adduser -S appuser -G appgroup
RUN mkdir -p /app/logs
RUN chown -R appuser:appgroup /app/logs
USER appuser

EXPOSE 8081
CMD ["./main"]