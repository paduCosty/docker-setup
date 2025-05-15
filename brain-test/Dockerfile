FROM php:8.2-apache

# Instalare extensii necesare pentru Laravel și dependențe adiționale
RUN apt-get update && apt-get install -y \
    libzip-dev \
    unzip \
    git \
    curl \
    libicu-dev \
    libonig-dev \
    libpng-dev \
    libjpeg-dev \
    libfreetype6-dev \
    libmagickwand-dev \
    libexif-dev \
    nodejs \
    npm \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install zip pdo_mysql bcmath pcntl gd intl exif \
    && pecl install imagick \
    && docker-php-ext-enable imagick \

# Activare module Apache
RUN a2enmod rewrite headers

# Instalare Node.js și npm
RUN curl -fsSL https://deb.nodesource.com/setup_20.x | bash - \
    && apt-get install -y nodejs \
    && npm install -g npm@latest

# Instalare Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin --filename=composer

# Setare director de lucru
WORKDIR /var/www/html

# Copiere fișiere aplicație
COPY ./brain-test /var/www/html

# Setare permisiuni pentru Laravel
RUN chown -R www-data:www-data /var/www/html \
    && chmod -R 775 /var/www/html/storage /var/www/html/bootstrap/cache

# Configurare VirtualHost pentru Apache
RUN echo '<VirtualHost *:80>\n\
    DocumentRoot /var/www/html/public\n\
    <Directory /var/www/html/public>\n\
        Options Indexes FollowSymLinks\n\
        AllowOverride All\n\
        Require all granted\n\
    </Directory>\n\
</VirtualHost>' > /etc/apache2/sites-available/000-default.conf

# Instalare pachete Node.js și build aplicația
RUN cd /var/www/html && npm install

# Expunere port
EXPOSE 80
