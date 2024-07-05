# Usa l'immagine di PHP 8.3.4 con FPM
FROM php:8.3.4-fpm

EXPOSE 8000
# Installa le dipendenze necessarie, inclusi i pacchetti di sviluppo di PostgreSQL
RUN apt-get update \
    && apt-get install -y \
        git \
        curl \
        libpng-dev \
        libjpeg-dev \
        libfreetype6-dev \
        zip \
        unzip \
        nginx \
        && apt-get install -y --no-install-recommends libpq-dev \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install -j$(nproc) gd pdo pdo_pgsql

# Install Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Set working directory
WORKDIR /var/www

# Copy and install Composer dependencies
COPY composer.json composer.lock ./
RUN composer install --prefer-dist --no-scripts --no-dev --no-autoloader

# Copy application source
COPY . .

# Change ownership of the application files
RUN chown -R www-data:www-data /var/www

# Copy Nginx configuration file
COPY ./nginx.conf /etc/nginx/nginx.conf

# Start PHP-FPM and Nginx
CMD ["php", "artisan", "serve", "--host=0.0.0.0", "--port=8000"]