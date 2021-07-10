FROM php:7.4-cli

RUN set -xe; \
    apt-get update && \
    apt-get install -y curl  zip  zlib1g-dev libzip-dev libicu-dev && \
    docker-php-ext-install zip intl

ARG APP_USER_ID=1000
ARG APP_GROUP_ID=1000

RUN set -xe; \
    groupadd -f workspace && \
    groupmod -g ${APP_GROUP_ID} workspace && \
    useradd workspace -g workspace && \
    mkdir -p /home/workspace && chmod 755 /home/workspace && chown workspace:workspace /home/workspace && \
    usermod -u ${APP_USER_ID} -m -d /home/workspace workspace -s $(which bash) && \
    chown -R workspace:workspace /var/www/html

RUN sed -i "s/upload_max_filesize = .*/upload_max_filesize = 16M/" /usr/local/etc/php/php.ini-development && \
    sed -i "s/post_max_size = .*/post_max_size = 16M/" /usr/local/etc/php/php.ini-development && \
    sed -i "s/max_execution_time = .*/max_execution_time = 10/" /usr/local/etc/php/php.ini-development && \
    sed -i "s/max_input_time = .*/max_input_time = 10/" /usr/local/etc/php/php.ini-development

RUN cp /usr/local/etc/php/php.ini-development /usr/local/etc/php/php.ini

ARG TIME_ZONE='Asia/Seoul'

RUN ln -snf /usr/share/zoneinfo/${TIME_ZONE} /etc/localtime && echo ${TIME_ZONE} > /etc/timezone

ARG COMPOSER_VERSION=2.0.2
ARG COMPOSER_REPO_PACKAGIST='https://packagist.jp'

ENV COMPOSER_ALLOW_SUPERUSER=1

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin --filename=composer --version=${COMPOSER_VERSION} && \
    composer config -g repos.packagist composer ${COMPOSER_REPO_PACKAGIST}

WORKDIR /var/www/html
