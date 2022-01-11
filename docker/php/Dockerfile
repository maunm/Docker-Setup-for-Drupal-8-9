FROM php:7.4-apache

RUN apt-get update && apt-get install -y default-mysql-client apt-utils git
RUN docker-php-ext-configure pdo_mysql --with-pdo-mysql=mysqlnd
RUN docker-php-ext-install mysqli pdo pdo_mysql opcache && docker-php-ext-enable mysqli pdo pdo_mysql

# Install some base extensions
RUN apt-get install -y \
        libzip-dev \
        zip \
  && docker-php-ext-install zip

RUN apt-get update

RUN apt-get update && apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install -j$(nproc) gd

RUN pecl install xdebug \
    && docker-php-ext-enable xdebug

# Install Memcached
RUN apt-get update
RUN apt-get install -y git libmemcached-dev wget unzip
RUN apt-get install -y libzip-dev zlib1g
RUN mkdir -p /usr/src/php/ext/memcached
WORKDIR /usr/src/php/ext/memcached
RUN wget https://github.com/php-memcached-dev/php-memcached/archive/v3.1.3.zip; unzip /usr/src/php/ext/memcached/v3.1.3.zip
RUN mv /usr/src/php/ext/memcached/php-memcached-3.1.3/* /usr/src/php/ext/memcached/
RUN docker-php-ext-configure memcached && docker-php-ext-install memcached 

# Enable mod_rewrite needed for clean URLs.
RUN a2enmod rewrite

# Set the main workiung dir.
WORKDIR /var/www

#Composer
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && php composer-setup.php --version=2.1.9 --install-dir=/usr/local/bin --filename=composer \
    && php -r "unlink('composer-setup.php');"

# Set the Drush version.
ENV DRUSH_VERSION 9.7.3

# Install Drush
RUN composer \
  --working-dir=/usr/src/ \
  global \
  require \
  drush/drush:10 && \
  ln -s /usr/src/vendor/bin/drush /usr/bin/drush10 && \
  ln -s /usr/src/vendor/bin/drush /usr/local/bin/drush
