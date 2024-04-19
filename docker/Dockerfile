ARG PHP_VERSION
FROM php:${PHP_VERSION}

ARG APP_DIR=/var/www/app

ARG REDIS_LIB=6.0.2

RUN apt-get update -y && apt-get install -y --no-install-recommends \ 
    apt-utils \ 
    supervisor

RUN apt-get update && apt-get install -y \
    zlib1g-dev \
    libzip-dev \
    unzip \
    libpng-dev \
    libpq-dev \
    libxml2-dev 

RUN docker-php-ext-install mysqli pdo pdo_mysql pdo_pgsql pgsql session xml

RUN pecl install redis-${REDIS_LIB} \ && docker-php-ext-enable redis

RUN docker-php-ext-install zip iconv simplexml pcntl gd fileinfo

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer 

# COPY ./docker/supervisord/supervisord.conf /etc/supervisor/conf.d/supervisord.conf
# COPY ./docker/supervisord/conf /etc/supervisord.d/

# COPY ./docker/php/extra-php.ini "$PHP_INI_DIR/99_extra.ini"
# COPY ./docker/php/extra-php-fpm.conf /etc/php8/php-fpm.d/www.conf

WORKDIR $APP_DIR
RUN cd $APP_DIR
RUN chown www-data:www-data $APP_DIR

COPY --chown=www-data:www-data ./App .
RUN rm -rf vendor
RUN composer install --no-interaction

RUN apt-get clean && rm -rf /var/lib/apt/list/*

CMD ["php", "-S", "0.0.0.0:8000", "-t", "public"]
# CMD [ "/usr/bin/supervisord", "-c", "/etc/supervisor/supervisord.conf" ]

EXPOSE 8000

