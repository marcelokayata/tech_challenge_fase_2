FROM golang:1.23-alpine AS builder

WORKDIR /src
COPY go.mod ./
RUN go mod download
COPY . .
RUN go mod tidy
RUN CGO_ENABLED=0 GOOS=linux go build -trimpath -ldflags="-s -w" -o /out/auth-service .

FROM alpine:3.21
RUN addgroup -S app && adduser -S -G app app
COPY --from=builder /out/auth-service /usr/local/bin/auth-service
USER app
EXPOSE 8001
ENTRYPOINT ["auth-service"]