FROM php:8.1.12-apache

## Update package information
RUN apt-get update

## Configure Apache
RUN a2enmod rewrite \
    && sed -i 's!/var/www/html!/var/www/html!g' /etc/apache2/sites-available/000-default.conf \
    && echo "AllowEncodedSlashes On" >> /etc/apache2/apache2.conf

## Install Composer
RUN curl -sS https://getcomposer.org/installer \
  | php -- --install-dir=/usr/local/bin --filename=composer

###
## PHP Extensisons
###

## Install zip libraries and extension
RUN apt-get install --yes git zlib1g-dev libzip-dev \
    && docker-php-ext-install zip

## Install intl library and extension
RUN apt-get install --yes libicu-dev \
    && docker-php-ext-configure intl \
    && docker-php-ext-install intl

###
## Optional PHP extensions
###

RUN apt-get install --yes libgmp-dev \
 && docker-php-ext-install gmp

## mbstring for i18n string support
RUN apt-get install --yes libonig-dev \
 && docker-php-ext-install mbstring

## gd extension for images
RUN apt-get install --yes libpng-dev \
    && docker-php-ext-configure gd \
        && docker-php-ext-install gd

## MySQL PDO support
 RUN docker-php-ext-install pdo_mysql \
    && docker-php-ext-install mysqli

## Redis support.  igbinary and libzstd-dev are only needed based on
## redis pecl options
 RUN pecl install igbinary \
     && docker-php-ext-enable igbinary \
     && apt-get install --yes libzstd-dev \
     && pecl install redis \
     && docker-php-ext-enable redis

## XDebug support
 RUN pecl install xdebug \
    && docker-php-ext-enable xdebug

## Disable access logs
RUN ln -fs /dev/null /var/log/apache2/access.log

WORKDIR /var/www/html
