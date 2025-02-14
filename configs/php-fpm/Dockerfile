FROM php:8.3-fpm

# Use the default production configuration
RUN mv "$PHP_INI_DIR/php.ini-production" "$PHP_INI_DIR/php.ini"

# Set working directory
WORKDIR /app/

# Set environment variables
ENV DEBIAN_FRONTEND noninteractive
ENV TZ=UTC

# Install system dependencies and clear cache
RUN apt -y update \
    && apt install -y \
    libfreetype6-dev \
    libpq-dev \
    libicu-dev \
    libjpeg62-turbo-dev \
    libpng-dev \
    libxml2-dev \
    libgmp-dev \
    libzip-dev \
    zip \
    unzip \
    && apt clean \
    && rm -rf /var/lib/apt/lists/*

# Install PHP extensions
RUN docker-php-ext-configure gd --with-freetype --with-jpeg
RUN docker-php-ext-install -j$(nproc) iconv gd pdo_pgsql soap bcmath gmp intl opcache zip exif

# Get latest Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Copy PHP config
# COPY php.ini /usr/local/etc/php/conf.d/zz-custom.ini