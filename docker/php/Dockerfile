FROM php:8.3-fpm

# Install dependencies and PHP extensions required by the app
RUN apt-get update && apt-get install -y \
    git \
    unzip \
    libzip-dev \
    libpng-dev \
    libonig-dev \
    libxml2-dev \
    libicu-dev \
    libjpeg62-turbo-dev \
    libfreetype6-dev \
    libssl-dev \
    zlib1g-dev \
    libcurl4-openssl-dev \
    pkg-config \
  && docker-php-ext-configure gd --with-freetype --with-jpeg \
  && docker-php-ext-install -j$(nproc) \
    bcmath \
    gd \
    intl \
    mbstring \
    opcache \
    pdo_mysql \
    zip \
  && rm -rf /var/lib/apt/lists/*

# Install Composer
WORKDIR /var/www/html

# Increase PHP memory limit via env var if provided
ENV PHP_MEMORY_LIMIT=512M
RUN { \
  echo "memory_limit=${PHP_MEMORY_LIMIT}"; \
  echo "upload_max_filesize=64M"; \
  echo "post_max_size=64M"; \
  echo "max_execution_time=120"; \
} > /usr/local/etc/php/conf.d/custom.ini

CMD ["php-fpm"]
CMD ["php-fpm"]


