#### Node image
FROM node:10.13.0 AS nodebase

RUN mkdir -p /var/www/html
WORKDIR /var/www/html
COPY . .

## Install node modules
RUN npm install

## Build static files
RUN npm run production


#### PHP image
FROM php:7.3-fpm-alpine AS base

## add necessary packages.
RUN apk --update add \
    && apk add git zlib-dev libzip-dev libpng-dev mysql-client \
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

#### Build for production.
FROM base AS prod

## Copy files to remote container.
WORKDIR /var/www/html
COPY . .

## Install Library
RUN composer global require laravel/installer \
    && composer install

## Import static files from Node image
COPY --from=nodebase /var/www/html/public/js/app.js    /var/www/html/public/js/app.js
COPY --from=nodebase /var/www/html/public/css/app.css  /var/www/html/public/css/app.css
COPY --from=nodebase /var/www/html/public/fonts/ /var/www/html/public/fonts/
COPY --from=nodebase /var/www/html/public/images/ /var/www/html/public/images/
COPY --from=nodebase /var/www/html/public/mix-manifest.json /var/www/html/public/mix-manifest.json

## Change directory owner setting
RUN /bin/chown www-data:www-data -R /var/www/html

EXPOSE 9000

#### Build for development.
# FROM base AS dev

## add development packages.
# RUN apk add autoconf gcc g++ make \
#     && pecl install xdebug \
#     && docker-php-ext-enable xdebug \
#     && apk add nodejs npm yarn