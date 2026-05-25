FROM golang:1.26-alpine3.23 AS builder

ARG GOOS=linux
ARG GOARCH=amd64
ARG CGO_ENABLED=0

WORKDIR /builder

COPY . .

ENV GOOS=$GOOS
ENV GOARCH=$GOARCH
ENV CGO_ENABLED=$CGO_ENABLED

RUN go mod download && go build -o my_app .

FROM alpine:3.20

WORKDIR /app

COPY --from=builder /builder/my_app .

RUN adduser -D -g "app" app_user 

USER app_user

EXPOSE 8081

CMD ["/app/"]

HEALTCHECK --interval=2s --start_period=10s --timeout=15s --retries=6 