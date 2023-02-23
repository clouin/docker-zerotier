ARG ALPINE_VERSION=edge

FROM alpine:${ALPINE_VERSION}

ARG ZT_VERSION=1.10.3-r0

RUN apk add --no-cache zerotier-one=${ZT_VERSION}

EXPOSE 9993/udp

COPY startup.sh /startup.sh
RUN chmod 0755 /startup.sh

ENTRYPOINT ["/startup.sh"]