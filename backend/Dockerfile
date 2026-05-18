# =========================================
# Stage 1: Build
# =========================================
FROM golang:1.17-alpine AS builder

WORKDIR /app

COPY go.mod go.sum ./
RUN go mod download

COPY . .

RUN CGO_ENABLED=0 GOOS=linux go build -o main ./cmd/api

# =========================================
# Stage 2: Runtime
# =========================================
FROM alpine:3.19

RUN addgroup -S apartgroup && adduser -S apartuser -G apartgroup

WORKDIR /app

COPY --from=builder --chown=apartuser:apartgroup /app/main .

USER apartuser

EXPOSE 8081

HEALTHCHECK --interval=30s --timeout=5s --start-period=5s --retries=3 \
  CMD wget --no-verbose --spider http://127.0.0.1:8081/health || exit 1

ENTRYPOINT ["./main"]