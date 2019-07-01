ARG DEBIAN_IMAGE=debian:jessie-slim
FROM $DEBIAN_IMAGE

RUN apt-get update \
    && apt-get install --no-install-recommends -y \
    gnupg \
    curl wget vim openssh-server software-properties-common netcat apachetop htop

RUN apt-get install --no-install-recommends -y build-essential apt-transport-https lsb-release ca-certificates -y
RUN wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg
RUN echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" > /etc/apt/sources.list.d/php.list

# Install all the PHP dependencies
RUN apt-get update -y && \
    apt-get install -y \
    mysql-client \
    php7.2-dev \
    php7.2  \
    php7.2-cli \
    php7.2-fpm \
    php7.2-common \
    php7.2-curl \
    php7.2-gd \
    php7.2-memcached \
    php7.2-xdebug \
    php-pear \
    php7.2-json \
    php7.2-mbstring \
    php7.2-intl \
    php7.2-mysql \
    php7.2-xml \
    php7.2-zip \
    php7.2-apcu \
    php7.2-ctype \
    php7.2-dom \
    php7.2-iconv \
    php7.2-imagick \
    php7.2-json \
    php7.2-intl \
    php7.2-opcache \
    php7.2-pdo \
    php7.2-mysqli \
    php7.2-xml  \
    php7.2-tokenizer \
    php7.2-zip \
    php7.2-simplexml \
    php7.2-bcmath \
    apache2 \
    libapache2-mod-fcgid \
    rsyslog --force-yes

RUN a2enmod actions && a2enmod proxy && a2enmod rewrite && a2enconf php7.2-fpm && a2enmod proxy_fcgi && rm -rf /var/lib/apt/lists/*

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php \
   && mv composer.phar /usr/local/bin/composer

# PHP Configuration
COPY docker-utils/php/php.ini /etc/php/7.2/fpm/php.ini
COPY docker-utils/php/pool.d/www.conf /etc/php/7.2/fpm/pool.d/www.conf

# Apache configurations
COPY docker-utils/apache2/app.conf /etc/apache2/sites-enabled/000-default.conf
# Disable Xdebug
RUN phpdismod xdebug

RUN chown www-data:www-data /var/www
EXPOSE 80
CMD service php7.2-fpm start && /usr/sbin/apache2ctl -D FOREGROUND
COPY docroot /var/www/html
RUN chown www-data:www-data /var/www
EXPOSE 80
CMD service php7.2-fpm start && /usr/sbin/apache2ctl -D FOREGROUND
