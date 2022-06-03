FROM php:7.4-fpm

# Arguments defined in docker-compose.yml
ARG user
ARG uid=1000

#RUN docker-php-ext-install

RUN apt-get update && apt-get install -y \
            git \
            libmemcached-dev \
            zlib1g-dev \
            memcached \
            unzip \
            librabbitmq-dev \
            libssh-dev \
            libzip-dev \
		    libjpeg-dev \
            libgmp-dev \
            libfreetype6-dev \
            libjpeg62-turbo-dev \
            libpng-dev \
        && docker-php-ext-configure gd --with-freetype --with-jpeg \
        && docker-php-ext-install -j$(nproc) \
            bcmath \
            sockets \
            zip \
            gd \
            gmp \
        && pecl install \
            amqp \
            memcached \
        && docker-php-ext-enable \
            amqp \
            memcached \
            gd \
        && apt-get purge -y \
		    libzip-dev \
		    libjpeg-dev 


COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

RUN useradd -G www-data,root -u $uid -d /home/$user $user
RUN mkdir -p /home/$user/.composer && \
    chown -R $user:$user /home/$user

USER $user

CMD ["php-fpm"]

EXPOSE 9000