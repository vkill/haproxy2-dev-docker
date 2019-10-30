# vim:set ft=dockerfile:
FROM alpine:3.10

RUN set -x \
    # && sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories \
    \
    && apk add --no-cache --virtual .haproxy-build-deps \
        ca-certificates \
        gcc \
        libc-dev \
        linux-headers \
        lua5.3-dev \
        make \
        openssl \
        openssl-dev \
        pcre2-dev \
        readline-dev \
        tar \
        zlib-dev \
    \
    && apk add --no-cache --virtual .vtest-run-deps \
        pcre-dev \
        varnish-dev \
    \
    && apk add --no-cache --virtual .vtest-build-deps \
        python3 \
    \
    && wget -O varnish.tar.gz https://github.com/varnishcache/varnish-cache/archive/varnish-6.3.1.tar.gz \
    && mkdir -p /usr/src/varnish \
	&& tar -xzf varnish.tar.gz -C /usr/src/varnish --strip-components=1 \
	&& rm varnish.tar.gz \
    \
    && wget -O VTest.tar.gz https://github.com/vtest/VTest/archive/master.tar.gz \
    && mkdir -p /usr/src/VTest \
	&& tar -xzf VTest.tar.gz -C /usr/src/VTest --strip-components=1 \
	&& rm VTest.tar.gz \
    \
    && cd /usr/src/VTest \
    && make vtest \
    && VARNISH_SRC=/usr/src/varnish make varnishtest \
    && cd \
    \
    && apk del --no-network .vtest-build-deps \
    && mv /usr/src/VTest/vtest /usr/local/bin \
    && mv /usr/src/VTest/varnishtest /usr/local/bin \
    && rm -rf /usr/src/varnish \
    && rm -rf /usr/src/VTest \
    \
    && mkdir -p /usr/src/haproxy \
    \
    && echo

ENV makeOpts ' \
    TARGET=linux-glibc \
    USE_GETADDRINFO=1 \
    USE_LUA=1 LUA_INC=/usr/include/lua5.3 LUA_LIB=/usr/lib/lua5.3 \
    USE_OPENSSL=1 \
    USE_PCRE2=1 USE_PCRE2_JIT=1 \
    USE_ZLIB=1 \
    \
    EXTRA_OBJS=" \
# see https://github.com/docker-library/haproxy/issues/94#issuecomment-505673353 for more details about prometheus support
        contrib/prometheus-exporter/service-prometheus.o \
    " \
'
ENV HAPROXY_PROGRAM /usr/src/haproxy/haproxy

WORKDIR /usr/src/haproxy
