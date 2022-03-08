FROM mysql:8.0 as database

# Default password for .env

ENV MYSQL_ROOT_PASSWORD=staging
ENV MYSQL_DATABASE=staging
ENV MYSQL_USER=staging
ENV MYSQL_PASSWORD=staging
EXPOSE 3306
RUN ls


FROM php:8-apache as webserver

ENV APACHE_DOCUMENT_ROOT /var/www/html/public

RUN sed -ri -e 's!/var/www/html!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/sites-available/*.conf
RUN sed -ri -e 's!/var/www/!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/apache2.conf /etc/apache2/conf-available/*.conf
COPY ./ /var/www/html/
# ADD sites/ /var/www/html/
#COPY ./my-httpd.conf /usr/local/apache2/conf/httpd.conf
#COPY ./config/000-default.conf /etc/apache2/sites-available/000-default.conf
#COPY ./config/apache2.conf /etc/apache2/apache2.conf
#COPY ./sites /usr/src/dst_folder
#WORKDIR /usr/src/dst_folder
RUN docker-php-ext-install pdo_mysql
RUN docker-php-ext-enable pdo_mysql

RUN apt-get update && apt-get install zip libzip-dev -y

RUN docker-php-ext-configure zip

WORKDIR /var/www/html
# RUN cp .env.example .env

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN composer update
RUN composer install

RUN php artisan cache:clear
RUN php artisan key:generate
RUN chown -R www-data:www-data /var/www/html/storage/
RUN chown -R www-data:www-data /var/www/html/bootstrap/cache
# RUN chmod -R g+s /var/www/html/storage/
# RUN chmod -R g+s /var/www/html/bootstrap/cache

#RUN docker-php-ext-configure zip --with-libzip
#RUN apt-get install -y apt-utils
#RUN apt-get install -y libfreetype6-dev \
#    libjpeg62-turbo-dev
#RUN docker-php-ext-configure gd --with-freetype --with-jpeg
#RUN docker-php-ext-install gd
#RUN docker-php-ext-configure gd --with-freetype --with-jpeg
#RUN apt-get update && apt-get install -y \
#    libfreetype6-dev \
#    libjpeg62-turbo-dev \
#    libpng-dev \
#    && docker-php-ext-configure gd --with-freetype --with-jpeg \
#    && docker-php-ext-install -j$(nproc) gd
# RUN docker-php-ext-install exif
# RUN a2enmod rewrite
# RUN service apache2 restart
EXPOSE 80
RUN echo hello world
