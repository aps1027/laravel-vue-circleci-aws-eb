# Laravel Vue Circleci AWS Elastic Beanstalk

## Prerequisites
1. install `docker`
1. install `docker-compose`

## Local Project Setup
1. start `docker-compose`
    ```
    $ docker-compose up -d
    ```
1. create `.env` file
    ```
    $ cp laravel/.env.example laravel/.env
    ```

1. install composer
    ```
    $ docker-compose exec php-fpm composer install
    ```

1. db migrate and seed
    ```
    $ docker-compose exec php-fpm php artisan migrate --seed
    ```

1. passport install
    ```
    $ docker-compose exec php-fpm php artisan passport:install
    ```

1. link to storage
    ```
    $ docker-compose exec php-fpm php artisan storage:link
    ```

1. give root permission to storage`(Only for linux user)`
    ```
    $ docker-compose exec php-fpm sh 
    $ cd storage && chown -R www-data:www-data *
    $ exit
    ```

1. watch vue changes
    ```
    $ docker-compose exec php-fpm yarn install 
    $ docker-compose exec php-fpm yarn watch
    ```
