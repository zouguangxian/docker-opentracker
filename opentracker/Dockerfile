
FROM alpine:latest as builder
LABEL maintainer="zouguangxian <zouguangxian@163.com>"
ARG S6_OVERLAY_VERSION=v1.21.4.0

RUN set -eux; \
    wget -O /tmp/s6-overlay-amd64.tar.gz https://github.com/just-containers/s6-overlay/releases/download/${S6_OVERLAY_VERSION}/s6-overlay-amd64.tar.gz \
    && tar xzf /tmp/s6-overlay-amd64.tar.gz -C / \
    && rm /tmp/*

RUN set -eux; \
    apk update \
    && apk add --no-cache cvs git \
    make binutils clang libc-dev gcc zlib-dev \
    && mkdir /build/ \
    && cd /build/ \
    && cvs -d :pserver:cvs@cvs.fefe.de:/cvs -z9 co libowfat \
    && cd libowfat \
    && make \
    && cd /build/ \
    && git clone git://erdgeist.org/opentracker \
    && cd opentracker \
    && make \
    && rm -rf /var/lib/apk/*

FROM alpine:latest
LABEL maintainer="zouguangxian <zouguangxian@163.com>"
ARG S6_OVERLAY_VERSION=v1.21.4.0

RUN set -eux; \
    wget -O /tmp/s6-overlay-amd64.tar.gz https://github.com/just-containers/s6-overlay/releases/download/${S6_OVERLAY_VERSION}/s6-overlay-amd64.tar.gz \
    && tar xzf /tmp/s6-overlay-amd64.tar.gz -C / \
    && rm /tmp/*

RUN set -eux; \
    apk update \
    && apk add --no-cache musl \
    && rm -rf /var/lib/apk/*

COPY --from=builder //build/opentracker/opentracker /usr/local/bin/

COPY rootfs /

ENTRYPOINT ["/init"]
