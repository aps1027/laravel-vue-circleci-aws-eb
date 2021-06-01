# Laravel Vue Circleci AWS Elastic Beanstalk
![alt text](https://github.com/aps1027/laravel-vue-circleci-aws-eb/blob/develop/doc/images/life_cycle.PNG?raw=true)

## Life Cycle
1. Do code changes
1. Commit and push code changes to GitHub
1. trigger code changes automatically via CircleCI and start testing (like UT and FT)
1. Start deployment on AWS after testing success

## Tools
1. Docker for Containerization
1. GitHub for Source Vesion Control
1. CircleCI for CI/CD tool
1. AWS services (Elastic Beanstalk)

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
1. access `localhost:8000`

## Automatic Deploy
### Elastic Beanstalk
1. create application with name `laravel-vue-circleci` and `LaravelVueCircleci-env`
1. select `docker` environment and select `64bit amazon linux`
1. set `mysql` database in the following:
    ![alt text](https://github.com/aps1027/laravel-vue-circleci-aws-eb/blob/develop/doc/images/eb_1.png?raw=true)
    ![alt text](https://github.com/aps1027/laravel-vue-circleci-aws-eb/blob/develop/doc/images/eb_2.png?raw=true)
1. after creation env:
    ![alt text](https://github.com/aps1027/laravel-vue-circleci-aws-eb/blob/develop/doc/images/eb_4.png?raw=true)
    ![alt text](https://github.com/aps1027/laravel-vue-circleci-aws-eb/blob/develop/doc/images/eb_3.png?raw=true)
1. add environment variables:
    ![alt text](https://github.com/aps1027/laravel-vue-circleci-aws-eb/blob/develop/doc/images/eb_8.png?raw=true)
### CircleCI
1. set up project `laravel-vue-circleci-aws-eb`
    ![alt text](https://github.com/aps1027/laravel-vue-circleci-aws-eb/blob/develop/doc/images/eb_5.png?raw=true)
1. select `existed config file`
    ![alt text](https://github.com/aps1027/laravel-vue-circleci-aws-eb/blob/develop/doc/images/eb_6.png?raw=true)
1. set `AWS_Environment_Variables`
    ![alt text](https://github.com/aps1027/laravel-vue-circleci-aws-eb/blob/develop/doc/images/eb_7.png?raw=true)
1. do code changes, commit and push
1. after, `test-build-deploy` will be `success`
    ![alt text](https://github.com/aps1027/laravel-vue-circleci-aws-eb/blob/develop/doc/images/eb_9.png?raw=true)

### Check your code change apply or not by accessing `<elastic beanstalk url>`.
    
