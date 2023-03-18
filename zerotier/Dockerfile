FROM alpine:edge

ARG ZT_VERSION=1.10.5-r0

RUN apk add --no-cache openssl-dev zerotier-one=${ZT_VERSION}

ENTRYPOINT ["/usr/sbin/zerotier-one"]
