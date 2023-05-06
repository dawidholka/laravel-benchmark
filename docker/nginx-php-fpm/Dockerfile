ARG COMPOSER_VERSION=latest

FROM composer:${COMPOSER_VERSION} AS vendor
FROM dwchiang/nginx-php-fpm:latest

COPY --from=composer /usr/bin/composer /usr/bin/composer

COPY public /usr/share/nginx/html
COPY . /var/www/html

RUN composer install --optimize-autoloader --no-dev --no-interaction --no-ansi --ignore-platform-reqs

RUN set -x && \
    # resolve: The stream or file "/var/www/html/storage/logs/laravel.log" could not be opened in append mode: failed to open stream: Permission denied
    touch /var/www/html/storage/logs/laravel.log && \
    touch /var/log/cron.log && \
    # please remove this APP_KEY generate for your production usage
    #   - ref: https://tighten.co/blog/app-key-and-you/
    chown -R www-data:www-data /usr/share/nginx/html && \
    chown -R www-data:www-data /var/www/html && \
    find /var/www/html/storage -type f -exec chmod 664 {} \; && \
    find /var/www/html/storage -type d -exec chmod 770 {} \;

CMD ["/docker-entrypoint.sh"]
