FROM php:8.1-fpm

# Install dependencies
RUN apt-get update && apt-get install -y \
    libpng-dev \
    libjpeg62-turbo-dev \
    libfreetype6-dev \
    locales \
    zip \
    jpegoptim optipng pngquant gifsicle \
    vim \
    unzip \
    git \
    curl \
    libpq-dev \
    libzip-dev \
    zlib1g-dev \
    libwebp-dev \
    libxpm-dev \
    libonig-dev \
    && docker-php-ext-configure gd --with-freetype --with-jpeg --with-webp --with-xpm \
    && docker-php-ext-install pdo_mysql pdo_pgsql mbstring exif pcntl bcmath gd \
    && pecl install redis \
    && docker-php-ext-enable redis \
    && apt-get clean && rm -rf /var/lib/apt/lists/*
# Установим Node.js и npm
RUN curl -fsSL https://deb.nodesource.com/setup_16.x | bash - \
    && apt-get install -y nodejs



# Установим Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Скопируем файл конфигурации PHP
COPY ../php/local.ini /usr/local/etc/php/conf.d/local.ini

WORKDIR /var/www

# Установим права на директории storage и bootstrap/cache
#RUN chown -R www-data:www-data /var/www/storage /var/www/bootstrap/cache
#RUN chmod -R 775 /var/www/storage /var/www/bootstrap/cache

# Установим зависимости Node.js и скомпилируем ресурсы (если нужно)
COPY package*.json ./
#RUN npm install

# Копируем весь проект в контейнер
COPY . .


# Запускаем PHP-FPM
CMD ["php-fpm"]