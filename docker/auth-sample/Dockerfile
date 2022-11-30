FROM php:8.1-apache

ENV COMPOSER_ALLOW_SUPERUSER=1 \
  COMPOSER_HOME=/composer

COPY --from=composer:2.0 /usr/bin/composer /usr/bin/composer

RUN apt-get update && \
apt-get install -y git wget zip unzip libmagickwand-dev fontconfig libzip-dev supervisor libonig-dev libfreetype6-dev libjpeg62-turbo-dev libpng-dev libjpeg-dev && \
docker-php-ext-install pdo pdo_mysql zip gd && \
docker-php-ext-install mysqli && docker-php-ext-enable mysqli && \
docker-php-ext-install mbstring && docker-php-ext-enable mbstring && \
docker-php-ext-configure gd --with-freetype --with-jpeg && \
docker-php-ext-install -j$(nproc) gd bcmath && \
pecl install imagick xdebug-3.1.1 && \
docker-php-ext-enable imagick xdebug

COPY 000-default.conf /etc/apache2/sites-available/
COPY ./php.ini /usr/local/etc/php/php.ini

RUN a2enmod rewrite

RUN curl -sL https://deb.nodesource.com/setup_16.x | bash -
RUN apt-get install -y nodejs
RUN npm install npm@latest -g

WORKDIR /var/www/auth-sample

COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf