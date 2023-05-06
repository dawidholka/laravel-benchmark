ARG PHP_VERSION=8.2
ARG COMPOSER_VERSION=latest
ARG ROADRUNNER_VERSION=2.12.3

# Dependencies
FROM composer:${COMPOSER_VERSION} AS vendor
FROM spiralscout/roadrunner:${ROADRUNNER_VERSION} as roadrunner
FROM php:${PHP_VERSION}-cli-alpine AS runtime

# Install composer
COPY --from=composer /usr/bin/composer /usr/bin/composer
# Install roadrunner
COPY --from=roadrunner /usr/bin/rr /usr/bin/rr

# Install required extensions
RUN apk --no-cache add socat curl linux-headers ${PHPIZE_DEPS} \
    && docker-php-ext-install ctype pcntl sockets pdo \
    && rm -rf /tmp/pear

# Setup document root
RUN mkdir -p /var/www/html

# Make sure files/folders needed by the processes are accessable when they run under the nobody user
RUN chown -R nobody.nobody /var/www/html && \
  chown -R nobody.nobody /run

#RUN ln -sf /proc/1/fd/1 /var/log/octane.log #this is now working

COPY docker/octane-roadrunner/php.ini "$PHP_INI_DIR/php.ini"

# Switch to use a non-root user from here on
USER nobody

# Add application
WORKDIR /var/www/html
COPY --chown=nobody . /var/www/html/

RUN rm -rf /var/www/html/node_modules

# Optimization
RUN composer install --optimize-autoloader --no-dev --no-interaction --no-ansi
# Install octane
RUN php artisan octane:install --server=roadrunner

EXPOSE 8080

ENV OCTANE_WORKERS 4

# Start octane
CMD php artisan octane:start --server=roadrunner --port=8080 --host="0.0.0.0" --workers=$OCTANE_WORKERS

HEALTHCHECK --start-period=5s --interval=2s --timeout=5s --retries=8 CMD php artisan octane:status || exit 1
