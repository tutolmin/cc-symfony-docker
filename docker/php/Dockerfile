FROM php:7.2-fpm

RUN docker-php-ext-install pdo_mysql bcmath opcache
# intl mbstring mysqlnd xml bcmath opcache

RUN pecl install apcu xdebug

RUN apt-get update && \
apt-get install -y \
zlib1g-dev unzip

RUN docker-php-ext-install zip
RUN docker-php-ext-enable apcu
RUN docker-php-ext-enable xdebug

RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && php composer-setup.php --filename=composer \
    && php -r "unlink('composer-setup.php');" \
    && mv composer /usr/local/bin/composer

WORKDIR /usr/src/app

COPY apps/symfony /usr/src/app
#RUN chown -R 1000:1000 /usr/src/app 

COPY docker/php/.env.local /usr/src/app/.env.local

RUN PATH=$PATH:/usr/src/apps/vendor/bin:bin
