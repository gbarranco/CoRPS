FROM php:fpm

ARG DOCKER_UID
ARG DOCKER_GID

RUN pecl install apcu

RUN apt-get update && \
apt-get install -y \
zlib1g-dev libzip-dev libpq-dev

RUN docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsql \
    && docker-php-ext-install pdo pdo_pgsql pgsql

RUN docker-php-ext-install zip
RUN docker-php-ext-enable apcu

RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && php composer-setup.php --filename=composer --install-dir=/usr/local/bin \
    && php -r "unlink('composer-setup.php');"

WORKDIR /var/www/html

RUN usermod -u $DOCKER_UID www-data && groupmod -g $DOCKER_GID www-data && chown -R www-data:www-data /var/www/html

RUN PATH=$PATH:/var/www/html/vendor/bin:bin
