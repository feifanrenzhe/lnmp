FROM php:7.4.3-fpm

ARG SYSTEM_TIMEZONE=Asia/ShangHai
RUN ln -snf /usr/share/zoneinfo/$SYSTEM_TIMEZONE /etc/localtime && \
    echo $SYSTEM_TIMEZONE > /etc/timezone

# libpng-devel
COPY ./sources.list /etc/apt/sources.list
RUN apt-get update -y && apt-get install -y sendmail git unzip libpng-dev libicu-dev libpq-dev libxml2-dev libzip-dev zlib1g-dev

# PHP扩展
RUN docker-php-ext-install -j$(nproc) \
    gd \
    intl \
    pdo_mysql \
    mysqli \
    pdo_pgsql \
    pgsql \
    soap \
    xml \
    xmlrpc \
    zip \
&& yes "" | pecl install xdebug \
&& docker-php-ext-enable xdebug


# Install Laravel composer
RUN php -r "readfile('http://getcomposer.org/installer');" | php -- --install-dir=/usr/bin/ --filename=composer
ENV COMPOSER_ALLOW_SUPERUSER 1
ENV COMPOSER_HOME /var/www/app/
ENV PATH $PATH:/var/www/app/vendor/bin
WORKDIR /var/www/app/

# Install logrotate
RUN apt-get install -y logrotate cron

#logrotate.d权限变更
RUN chmod 644 -R /etc/logrotate.d/; chown root:root -R /etc/logrotate.d/; 

#自启动设置
#COPY ./entrypoint.sh /
#RUN chmod +x /entrypoint.sh
#ENTRYPOINT /entrypoint.sh
