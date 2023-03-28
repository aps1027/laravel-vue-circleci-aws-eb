FROM php:7.3-fpm-alpine AS base

## add necessary packages.
RUN apk --update add \
    && apk add git zlib-dev libzip-dev libpng-dev \
    && docker-php-ext-install pdo_mysql zip gd \
    && apk add nodejs npm yarn

RUN apk add gnu-libiconv --update-cache --repository http://dl-cdn.alpinelinux.org/alpine/edge/community/ --allow-untrusted

ENV LD_PRELOAD /usr/lib/preloadable_libiconv.so php


## Install composer.
COPY --from=composer /usr/bin/composer /usr/local/bin/composer

## Set composer valiables.
ENV COMPOSER_ALLOW_SUPERUSER 1
ENV COMPOSER_HOME /composer
ENV PATH $PATH:/composer/vendor/bin

## Download laravel installer.
RUN composer global require laravel/installer

#### Build for production.
# FROM base AS prod

## Copy files.
# WORKDIR /var/www/html/base
# COPY . .

# EXPOSE 9000

#### Build for development.
FROM base AS dev

## add development packages.
RUN apk add autoconf gcc g++ make \
    && pecl install xdebug-2.9.8 \
    && docker-php-ext-enable xdebug \
    && apk add nodejs npm yarn
