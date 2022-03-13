FROM php:8.0-apache
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer
RUN apt-get update && \
    apt-get -y install git zip unzip && \
    docker-php-ext-install pdo_mysql
    
RUN sed -i 's!/var/www/html!/var/www/app/public!g' /etc/apache2/sites-available/000-default.conf
RUN a2enmod rewrite
