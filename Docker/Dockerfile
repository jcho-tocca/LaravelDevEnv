FROM php:7.3.3-apache 
RUN apt-get update && apt-get upgrade -y 
RUN docker-php-ext-install mysqli 
RUN pecl install xdebug-2.9.6 \
    && docker-php-ext-enable xdebug
EXPOSE 80