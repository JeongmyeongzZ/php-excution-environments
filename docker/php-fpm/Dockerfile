FROM php:7.4.2-fpm-buster

RUN set -xe; \
    apt-get update -yqq && \
    pecl channel-update pecl.php.net && \
    apt-get install -yqq \
      apt-utils \
      libzip-dev zip unzip && \
    docker-php-ext-install zip pdo_mysql

ARG APP_USER_ID=1000
ARG APP_GROUP_ID=1000

RUN set -xe; \
    groupadd -f app && \
    groupmod -g ${APP_GROUP_ID} app && \
    useradd app -g app && \
    mkdir -p /home/app && chmod 755 /home/app && chown app:app /home/app && \
    usermod -u ${APP_USER_ID} -m -d /home/app app -s $(which bash) && \
    chown -R app:app /var/www/html

RUN sed -i "s/upload_max_filesize = .*/upload_max_filesize = 16M/" /usr/local/etc/php/php.ini-development && \
    sed -i "s/post_max_size = .*/post_max_size = 16M/" /usr/local/etc/php/php.ini-development && \
    sed -i "s/max_execution_time = .*/max_execution_time = 3/" /usr/local/etc/php/php.ini-development && \
    sed -i "s/max_input_time = .*/max_input_time = 3/" /usr/local/etc/php/php.ini-development

RUN cp /usr/local/etc/php/php.ini-development /usr/local/etc/php/php.ini

# Set Timezone
ARG TZ=UTC
ENV TZ ${TZ}

RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
