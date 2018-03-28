FROM eccube/php7-ext-codeception


### see also https://hub.docker.com/r/codeception/codeception/~/dockerfile/

RUN echo "date.timezone = Asia/Tokyo" >> /usr/local/etc/php/php.ini

## Add mailcatcher-codeception-module
WORKDIR /repo
RUN composer require captbaritone/mailcatcher-codeception-module "1.*"

WORKDIR /project

ENTRYPOINT ["docker-php-entrypoint"]

CMD ["tail", "-f", "/dev/null"]