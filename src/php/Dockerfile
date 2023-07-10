FROM php:8.1-fpm-alpine
RUN apk update && apk add linux-headers libpq-dev zip libzip-dev libpng-dev libwebp-dev jpeg-dev && docker-php-ext-install pdo pdo_pgsql
RUN php -r "readfile('https://getcomposer.org/installer');" | php -- --install-dir=/usr/bin/ --filename=composer
RUN apk --no-cache add yaml-dev pcre-dev ${PHPIZE_DEPS}  \
    && pear channel-update pear.php.net && pecl channel-update pecl.php.net \
    && pecl install xdebug yaml \
    && docker-php-ext-configure gd --with-webp --with-jpeg  \
    && docker-php-ext-install zip gd \
    && docker-php-ext-enable xdebug gd yaml \
    && apk del pcre-dev ${PHPIZE_DEPS}

ARG UID=1001
ARG GID=1001
RUN apk --no-cache add shadow && \
    usermod -u ${UID} www-data && \
    groupmod -g ${GID} www-data
COPY docker/src/php/my.ini /usr/local/etc/php/conf.d/my.ini
USER www-data
