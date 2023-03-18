FROM alpine:edge

ARG ZT_VERSION=1.10.5-r0

RUN apk add --no-cache openssl-dev zerotier-one=${ZT_VERSION}

EXPOSE 9993/udp

COPY startup.sh /usr/bin/
RUN chmod +x /usr/bin/startup.sh

ENTRYPOINT ["startup.sh"]
