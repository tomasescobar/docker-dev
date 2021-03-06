FROM php:5.6-fpm

# Installing tools and PHP extentions
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        curl \
        libmemcached-dev \
        libz-dev \
        libpq-dev \
        libjpeg-dev \
        libpng12-dev \
        libfreetype6-dev \
        libssl-dev \
        libmcrypt-dev \
        git \
        unzip

# Install the PHP mcrypt, pdo_mysql, pdo_pgsql extention, PHP gd library
RUN docker-php-ext-install mcrypt \
    && docker-php-ext-install mysqli \
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-install pdo_pgsql \
    && docker-php-ext-install gd \
    && docker-php-ext-configure gd \
        --enable-gd-native-ttf \
        --with-jpeg-dir=/usr/lib \
        --with-freetype-dir=/usr/include/freetype2 \
    && docker-php-ext-install gd

# Memcached
RUN apt-get update && apt-get install -y libmemcached11 libmemcachedutil2 build-essential libmemcached-dev libz-dev \
    && pecl install memcached-2.2.0 \
    && echo extension=memcached.so >> /usr/local/etc/php/conf.d/memcached.ini

# Add conf
ADD ./laravel.ini /usr/local/etc/php/conf.d/
ADD ./pool.conf /usr/local/etc/php-fpm.d/

RUN usermod -u 1000 www-data

WORKDIR /var/www/api

# Download composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/var/www/api --filename=composer

# Run composer in workbench directories
ENV COMPOSER_ALLOW_SUPERUSER 1
# RUN ./composer install --no-interaction -d /var/www/api/workbench/acamica/analytics
# RUN ./composer install --no-interaction -d /var/www/api/workbench/acamica/authorize
# RUN ./composer install --no-interaction -d /var/www/api/workbench/acamica/gamify
# RUN ./composer install --no-interaction -d /var/www/api/workbench/acamica/openid-client
# RUN ./composer install --no-interaction -d /var/www/api/workbench/tomasescobar/astropay
# RUN ./composer install --no-interaction -d /var/www/api/workbench/tomasescobar/maily

# RUN ./composer install --no-interaction --ignore-platform-reqs

CMD ["php-fpm"]

EXPOSE 9000