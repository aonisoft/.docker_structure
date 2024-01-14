ARG IMAGE_NAME_PHP=

FROM ${IMAGE_NAME_PHP}

RUN apk --update --no-cache upgrade

RUN apk add --no-cache autoconf automake make gcc g++ \ 
                       git bash icu-dev libzip-dev rabbitmq-c \
                       rabbitmq-c-dev linux-headers           \
                       icu-data-full libpng-dev 

RUN pecl install apcu && pecl install amqp && pecl install xdebug 

RUN docker-php-ext-install -j$(nproc) bcmath opcache intl zip pdo_mysql gd

RUN docker-php-ext-enable amqp apcu gd pdo_mysql zip intl opcache

RUN curl -s https://getcomposer.org/installer | php

RUN mv composer.phar /usr/local/bin/composer

WORKDIR /website

CMD [ "php-fpm" ]