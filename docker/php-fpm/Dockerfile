FROM php:fpm-alpine

COPY wait-for-it.sh /usr/bin/wait-for-it

RUN chmod +x /usr/bin/wait-for-it

# git
RUN apk --update --no-cache add git

# pdo_mysql
RUN docker-php-ext-install pdo_mysql

# intl
RUN apk add --update --no-cache libintl icu icu-dev \
    && docker-php-ext-configure intl \
    && docker-php-ext-install intl

# zip
RUN apk add --update --no-cache zip libzip-dev \
    && docker-php-ext-configure zip \
    && docker-php-ext-install zip

# mcrypt, gd, iconv

# imagick

# xdebug
RUN apk add --no-cache $PHPIZE_DEPS \
    && pecl install xdebug-stable \
    && docker-php-ext-enable xdebug

COPY --from=composer /usr/bin/composer /usr/bin/composer

WORKDIR /var/www

CMD composer install ; wait-for-it database:3306 -- bin/console doctrine:migrations:migrate ;  php-fpm 

EXPOSE 9000
