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
1. access `localhost:8000`

## Automatic Deploy
### Elastic Beanstalk
1. create application with name `laravel-vue-circleci` and `LaravelVueCircleci-env`
1. select `docker` environment and select `64bit amazon linux`
1. set `mysql` database in the folowing:
    ![alt text](https://github.com/aps1027/k8s-jenkins-ci-cd/blob/develop/doc/images/eb_1.png?raw=true)
    ![alt text](https://github.com/aps1027/k8s-jenkins-ci-cd/blob/develop/doc/images/eb_2.png?raw=true)
1. after creation env:
    ![alt text](https://github.com/aps1027/k8s-jenkins-ci-cd/blob/develop/doc/images/eb_4.png?raw=true)
    ![alt text](https://github.com/aps1027/k8s-jenkins-ci-cd/blob/develop/doc/images/eb_3.png?raw=true)
1. add environment variables:
    ![alt text](https://github.com/aps1027/k8s-jenkins-ci-cd/blob/develop/doc/images/eb_8.png?raw=true)
### CircleCI
1. set up project `laravel-vue-circleci-aws-eb`
    ![alt text](https://github.com/aps1027/k8s-jenkins-ci-cd/blob/develop/doc/images/eb_5.png?raw=true)
1. select `existed config file`
    ![alt text](https://github.com/aps1027/k8s-jenkins-ci-cd/blob/develop/doc/images/eb_6.png?raw=true)
1. set `AWS_Environment_Variables`
    ![alt text](https://github.com/aps1027/k8s-jenkins-ci-cd/blob/develop/doc/images/eb_7.png?raw=true)
1. do code changes, commit and push
1. after, `test-build-deploy` will be `success`

### Check your code change apply or not by accessing `<elastic beanstalk url>`.
    
