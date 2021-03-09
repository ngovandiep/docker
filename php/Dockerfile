ARG PHP_VERSION
FROM php:${PHP_VERSION}-fpm

RUN apt-get update && apt-get install -y libmcrypt-dev git curl libzip-dev libpng-dev zip unzip vim openssl \
    libmagickwand-dev --no-install-recommends ssh\
    && pecl install imagick \
    && docker-php-ext-enable imagick \
    && docker-php-ext-install gd mbstring pdo pdo_mysql exif mbstring zip

RUN \
    apt-get update && \
    apt-get install libldap2-dev -y && \
    rm -rf /var/lib/apt/lists/* && \
    docker-php-ext-configure ldap --with-libdir=lib/x86_64-linux-gnu/ && \
    docker-php-ext-install ldap

RUN pecl install redis && docker-php-ext-enable redis

COPY config /usr/local/etc/php
# Install composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
# Set permissions
ARG VOLUME_PATH
RUN echo ${VOLUME_PATH}
RUN mkdir ${VOLUME_PATH}
RUN chown -R www-data:www-data ${VOLUME_PATH}