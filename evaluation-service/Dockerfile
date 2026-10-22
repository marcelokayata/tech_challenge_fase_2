FROM golang:1.23-alpine AS builder

WORKDIR /src
COPY go.mod ./
RUN go mod download
COPY . .
RUN go mod tidy
RUN CGO_ENABLED=0 GOOS=linux go build -trimpath -ldflags="-s -w" -o /out/evaluation-service .

FROM alpine:3.21
RUN addgroup -S app && adduser -S -G app app
COPY --from=builder /out/evaluation-service /usr/local/bin/evaluation-service
USER app
EXPOSE 8004
ENTRYPOINT ["evaluation-service"]