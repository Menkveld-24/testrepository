
FROM php:8-apache as webserver

ENV APACHE_DOCUMENT_ROOT /var/www/html/public

RUN sed -ri -e 's!/var/www/html!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/sites-available/*.conf
RUN sed -ri -e 's!/var/www/!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/apache2.conf /etc/apache2/conf-available/*.conf
COPY ./ /var/www/html/

RUN docker-php-ext-install pdo_mysql
RUN docker-php-ext-enable pdo_mysql

RUN apt-get update && apt-get install zip libzip-dev -y

RUN docker-php-ext-configure zip

WORKDIR /var/www/html
RUN ls

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

RUN a2enmod rewrite
RUN service apache2 restart
