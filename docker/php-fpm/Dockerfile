FROM php:7.4-fpm-alpine

ARG APP_USER_ID=1000
ARG APP_GROUP_ID=1000

RUN set -xe; \
    apk --update add \
        tzdata && \
        rm /var/cache/apk/*

ARG TIME_ZONE='Asia/Seoul'

RUN cp /usr/share/zoneinfo/${TIME_ZONE} /etc/localtime
