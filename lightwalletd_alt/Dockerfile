FROM golang:1.13 AS lightwalletd_base

RUN git clone --depth 1 \
  https://github.com/adityapk00/lightwalletd.git \
  /go/src/github.com/zcash/lightwalletd
WORKDIR /go/src/github.com/zcash/lightwalletd
RUN CGO_ENABLED=0 go build -a -ldflags '-extldflags "-static"' cmd/server/main.go

FROM debian:10

COPY --from=lightwalletd_base \
  /go/src/github.com/zcash/lightwalletd/main \
  /usr/bin/server

ARG LWD_USER=lightwalletd
ARG LWD_UID=2002

RUN useradd --home-dir /srv/$LWD_USER \
            --shell /bin/bash \
            --create-home \
            --uid $LWD_UID\
            $LWD_USER
USER $LWD_USER
WORKDIR /srv/$LWD_USER

ENTRYPOINT ["server"]
CMD ["--help"]