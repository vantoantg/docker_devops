FROM php:7.1-apache
RUN apt-get update && apt-get install -y gnupg2

RUN touch /etc/apt/sources.list.d/tideways.list
RUN echo "deb http://s3-eu-west-1.amazonaws.com/qafoo-profiler/packages debian main" > /etc/apt/sources.list.d/tideways.list

RUN apt-get update -y && apt install -y apt-utils wget

RUN  wget -qO - https://s3-eu-west-1.amazonaws.com/qafoo-profiler/packages/EEB5E8F4.gpg | apt-key add -

RUN apt-get update -y && apt-get install -y zip unzip curl libfreetype6-dev libjpeg62-turbo-dev libmcrypt-dev \
       libpng-dev zlib1g-dev libicu-dev g++ libcurl4-gnutls-dev libxml2-dev libpq-dev \
       tideways-daemon tideways-php tideways-cli

RUN docker-php-ext-install -j$(nproc) iconv mcrypt gd intl bcmath mbstring curl soap mysqli pdo pdo_mysql pdo_pgsql opcache zip

RUN pecl install redis mongodb
RUN pecl install redis xdebug
RUN docker-php-ext-enable redis xdebug

RUN curl -sS https://getcomposer.org/installer | php -- --filename=composer --install-dir=/usr/local/bin

RUN chown -R www-data: /var/www

USER www-data

RUN composer global require --optimize-autoloader "hirak/prestissimo"

USER root

RUN touch /usr/local/etc/php/conf.d/custom.ini

RUN echo "ServerName localhost" | tee /etc/apache2/conf-available/fqdn.conf
RUN mkdir -p /etc/apache2/vhosts
RUN echo "IncludeOptional vhosts/*.conf" >> /etc/apache2/apache2.conf

RUN a2enconf fqdn
RUN a2enmod rewrite
RUN a2enmod deflate
RUN service apache2 restart
