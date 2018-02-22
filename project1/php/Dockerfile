FROM php:7.1-fpm

RUN apt-get update \
    && apt-get -y install \
            libmagickwand-dev \
            ghostscript \
            libmcrypt-dev \
            libldap2-dev \
            git \
            unzip \
            autoconf \
  	        file \
   	        g++ \
  	        gcc \
  	        libc-dev \
  	        make \
  	        pkg-config \
  	        re2c \
  	        zlib1g-dev \
        --no-install-recommends \
    && pecl install imagick \
    && docker-php-ext-enable imagick \
    && docker-php-ext-install pdo pdo_mysql \
    && docker-php-ext-install gd \
    && docker-php-ext-install zip \
    && cd /tmp && curl -Ls http://download.icu-project.org/files/icu4c/$ICU_RELEASE/icu4c-$(echo $ICU_RELEASE | tr '.' '_')-src.tgz > icu4c-src.tgz \
    && cd /tmp && tar xzf icu4c-src.tgz && cd /tmp/icu/source && ./configure && make && make install && rm -rf /tmp/icu /tmp/icu4c-src.tgz \
    && docker-php-ext-configure intl && docker-php-ext-install intl \
    && docker-php-ext-install mcrypt \
    && pecl install -o -f redis \
    && rm -rf /tmp/pear \
    && docker-php-ext-enable redis \
    && echo "mbstring.internal_encoding = UTF-8" > /usr/local/etc/php/php.ini

#install composer
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
  && php -r "if (hash_file('SHA384', 'composer-setup.php') === '544e09ee996cdf60ece3804abc52599c22b1f40f4323403c44d44fdfdd586475ca9813a858088ffbc1f233e9b180f061') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" \
  && php composer-setup.php --install-dir=/usr/local/bin --filename=composer \
  && php -r "unlink('composer-setup.php');" \
  && composer global require hirak/prestissimo

RUN yes | pecl install xdebug \
    && echo "zend_extension=$(find /usr/local/lib/php/extensions/ -name xdebug.so)" > /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_enable=on" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_autostart=off" >> /usr/local/etc/php/conf.d/xdebug.ini

EXPOSE 80

