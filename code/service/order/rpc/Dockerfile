FROM golang:alpine AS builder

LABEL stage=gobuilder

ENV CGO_ENABLED 0
ENV GOOS linux
ENV GOPROXY https://goproxy.cn,direct

WORKDIR /build/zero

ADD go.mod .
ADD go.sum .
RUN go mod download
COPY . .
COPY service/order/rpc/etc /app/etc
RUN go build -ldflags="-s -w" -o /app/order service/order/rpc/order.go


FROM alpine

RUN apk update --no-cache && apk add --no-cache ca-certificates tzdata
ENV TZ Asia/Shanghai

WORKDIR /app
COPY --from=builder /app/order /app/order
COPY --from=builder /app/etc /app/etc

CMD ["./order", "-f", "etc/order.prod.yaml"]
