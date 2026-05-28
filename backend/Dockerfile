FROM golang:1.26-alpine3.23 AS builder

ARG GOOS=linux
ARG GOARCH=amd64
ARG CGO_ENABLED=0

WORKDIR /builder

COPY ./go.mod ./go.sum ./

RUN go mod download

ENV GOOS=$GOOS
ENV GOARCH=$GOARCH
ENV CGO_ENABLED=$CGO_ENABLED

COPY ./ ./

RUN go build -o my_app ./cmd/api




FROM alpine:3.20

WORKDIR /app

COPY --from=builder /builder/my_app ./my_app

RUN addgroup -S "app" && \
    adduser -SDH -g "app" -s /sbin/nologin app_user && \
    chown app_user:app ./my_app && \
    chmod u+x,g+x ./my_app

USER app_user

EXPOSE 8081

CMD ["/app/my_app"]

HEALTHCHECK --interval=45s --start-period=10s --timeout=10s --retries=5 CMD wget --spider -q http://localhost:8081/health/