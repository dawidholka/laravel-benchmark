ARG PHP_VERSION=8.2
ARG COMPOSER_VERSION=latest

# Dependencies
FROM composer:${COMPOSER_VERSION} AS vendor
FROM php:${PHP_VERSION}-cli-alpine AS runtime

# Install composer
COPY --from=composer /usr/bin/composer /usr/bin/composer

# Install required extensions
RUN apk --no-cache add socat curl linux-headers ${PHPIZE_DEPS} \
    && docker-php-ext-install ctype pcntl sockets pdo \
    && pecl install swoole \
    && docker-php-ext-enable swoole \
    && rm -rf /tmp/pear


# Setup document root
RUN mkdir -p /var/www/html

RUN chown -R nobody.nobody /var/www/html && \
  chown -R nobody.nobody /run

USER nobody

WORKDIR /var/www/html
COPY --chown=nobody . /var/www/html/

# Configure PHP
COPY docker/octane-swoole/php.ini "$PHP_INI_DIR/php.ini"

RUN composer install --optimize-autoloader --no-dev --no-interaction --no-ansi

RUN php artisan octane:install --server=swoole

EXPOSE 8080

ENV OCTANE_WORKERS 4

CMD php artisan octane:start --server=swoole --port=8080 --host="0.0.0.0" --workers=$OCTANE_WORKERS

HEALTHCHECK --start-period=5s --interval=2s --timeout=5s --retries=8 CMD php artisan octane:status || exit 1
