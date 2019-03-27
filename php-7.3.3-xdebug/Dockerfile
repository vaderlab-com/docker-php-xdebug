FROM php:7.3.3-fpm-stretch

RUN apt-get update
RUN apt-get install -y curl autoconf build-essential nano

RUN curl -fsSL 'https://xdebug.org/files/xdebug-2.7.0.tgz' -o xdebug.tar.gz \
    && mkdir -p xdebug \
    && tar -xf xdebug.tar.gz -C xdebug --strip-components=1 \
    && rm xdebug.tar.gz \
    && ( \
    cd xdebug \
    && phpize \
    && ./configure --enable-xdebug \
    && make -j$(nproc) \
    && make install \
    ) \
    && rm -r xdebug \
    && docker-php-ext-enable xdebug

RUN apt-get remove -y --purge autoconf build-essential \
    && apt-get autoremove -y \
    && apt-get clean


RUN curl -fsSL 'https://getcomposer.org/installer' -o composer-setup.php \
    && php composer-setup.php --filename=composer --install-dir=/bin \
    && rm composer-setup.php

WORKDIR "/app"