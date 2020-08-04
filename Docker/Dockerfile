FROM php:7.3.3-apache 
RUN apt-get update && apt-get upgrade -y 
RUN docker-php-ext-install mysqli pdo pdo_mysql
RUN pecl install xdebug-2.9.6 \
    && docker-php-ext-enable xdebug
COPY ./apache/000-default.conf /etc/apache2/sites-available/000-default.conf 
COPY ./php/php.ini /usr/local/etc/php/php.ini
EXPOSE 80