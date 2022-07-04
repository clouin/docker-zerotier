ARG ALPINE_VERSION=edge

FROM alpine:${ALPINE_VERSION}

ARG ZT_VERSION=1.10.0-r0

RUN apk add --no-cache zerotier-one=${ZT_VERSION}

ENTRYPOINT ["/usr/sbin/zerotier-one"]