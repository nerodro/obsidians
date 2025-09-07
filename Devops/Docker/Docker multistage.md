```
FROM golang:1.21 AS builder
WORKDIR /app
COPY go.mod go.sum ./
RUN go mod download

COPY . ./

RUN CGO_ENABLED=0 GOOS=linux go build -o /my-app ./cmd/app

FROM alpine:3.18
RUN apk --no-cache add ca-certificates
COPY --from=builder /my-app /usr/local/bin/my-app
CMD ["my-app"]
```
