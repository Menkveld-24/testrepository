FROM php:8-apache as webserver

ENV APACHE_DOCUMENT_ROOT /var/www/html/public

RUN sed -ri -e 's!/var/www/html!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/sites-available/*.conf
RUN sed -ri -e 's!/var/www/!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/apache2.conf /etc/apache2/conf-available/*.conf
COPY ./ /var/www/html/

RUN docker-php-ext-install pdo_mysql
RUN docker-php-ext-enable pdo_mysql

RUN apt-get update && apt-get install zip libzip-dev nano -y

RUN docker-php-ext-configure zip

WORKDIR /var/www/html
RUN ls

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

RUN composer update
RUN composer install

RUN php artisan cache:clear
RUN php artisan key:generate
RUN chown -R www-data:www-data /var/www/html/storage/
RUN chown -R www-data:www-data /var/www/html/bootstrap/cache

RUN a2enmod rewrite
RUN service apache2 restart
