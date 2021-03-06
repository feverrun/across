# Dockerfile for MTProxy based alpine
# Copyright (C) 2020 Teddysun <i@teddysun.com>
# Reference URL:
# https://github.com/TelegramMessenger/MTProxy

FROM alpine:latest AS builder
WORKDIR /root
COPY patches /root/patches
RUN set -ex \
	&& apk add --no-cache git build-base linux-headers musl-dev openssl-dev zlib-dev \
	&& git clone https://github.com/TelegramMessenger/MTProxy.git \
	&& cd MTProxy \
	&& patch -p0 -i /root/patches/randr_compat.patch \
	&& make

FROM alpine:latest
LABEL maintainer="Teddysun <i@teddysun.com>"

COPY entrypoint.sh /
COPY --from=builder /root/MTProxy/objs/bin/mtproto-proxy /usr/bin

RUN set -ex \
	&& apk add --no-cache curl tzdata \
	&& chmod 755 /entrypoint.sh
ENV TZ=Asia/Shanghai
EXPOSE 443 2398
VOLUME /data
WORKDIR /data
ENTRYPOINT ["/entrypoint.sh"]
CMD [ "--port", "2398", "--http-ports", "443", "--slaves", "2", "--max-special-connections", "60000", "--allow-skip-dh" ]
