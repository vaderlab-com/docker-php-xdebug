FROM php:7.3.3-fpm-stretch

RUN apt-get update
RUN apt-get install -y --no-install-recommends curl autoconf build-essential git nano zlib1g-dev libzip-dev zip unzip

RUN curl -sS -o /tmp/icu.tar.gz -L http://download.icu-project.org/files/icu4c/63.1/icu4c-63_1-src.tgz \
    && tar -zxf /tmp/icu.tar.gz -C /tmp && cd /tmp/icu/source \
    && ./configure --prefix=/usr/local --enable-tests=no --enable-samples=no \
    && make && make install

RUN curl -fsSL 'https://xdebug.org/files/xdebug-2.7.0.tgz' -o /tmp/xdebug.tar.gz \
    && mkdir -p /tmp/xdebug \
    && tar -xf /tmp/xdebug.tar.gz -C /tmp/xdebug --strip-components=1 \
    && ( \
    cd /tmp/xdebug \
    && phpize \
    && ./configure --enable-xdebug \
    && make -j$(nproc) \
    && make install \
    ) \
    && docker-php-ext-enable xdebug

RUN docker-php-ext-install zip intl

RUN apt-get remove -y --purge autoconf build-essential \
    && apt-get autoremove -y \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/*


ARG XDEBUG_ENABLED=1
ARG XDEBUG_REMOTE_PORT=9001
ARG XDEBUG_REMOTE_HOST="10.0.75.1"
ARG XDEBUG_IDEKEY="PHPSTORM"
ARG XDEBUG_REMOTE_AUTOSTART=1
ARG XDEBUG_SERVERNAME="Vaderlab-docker"
ARG PHP_XDEBUG_IDE_CONFIG="serverName=${XDEBUG_SERVERNAME}"
ARG XDEBUG_ENABLED=1

ENV PHP_IDE_CONFIG=$PHP_XDEBUG_IDE_CONFIG
ENV PHP_XDEBUG_ENABLED=$XDEBUG_ENABLED
ENV PHP_XDEBUG_REMOTE_PORT=$XDEBUG_REMOTE_PORT
ENV PHP_XDEBUG_REMOTE_HOST=$XDEBUG_REMOTE_HOST
ENV PHP_XDEBUG_IDEKEY=$XDEBUG_IDEKEY
ENV PHP_XDEBUG_REMOTE_AUTOSTART=$XDEBUG_REMOTE_AUTOSTART

ENV XDEBUG_CONFIG="remote_enable=$PHP_XDEBUG_ENABLED \
    remote_port=$PHP_XDEBUG_REMOTE_PORT \
    remote_host=$PHP_XDEBUG_REMOTE_HOST \
    idekey=$PHP_XDEBUG_IDEKEY \
    extended_info=1 \
    remote_autostart=$PHP_XDEBUG_REMOTE_AUTOSTART \
    remote_connect_back=Off"

RUN mv "$PHP_INI_DIR/php.ini-development" "$PHP_INI_DIR/php.ini"

WORKDIR "/app"