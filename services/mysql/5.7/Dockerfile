FROM alpine:3.7

MAINTAINER yinfuyuan <yinfuyuan@gmail.com>

RUN addgroup -S mysql && adduser -S -G mysql mysql

RUN apk add --no-cache 'su-exec>=0.2'

RUN mkdir /docker-entrypoint-initdb.d

RUN apk update; \
    apk add openrc pwgen openssl; \
    rm -rf /tmp/* /var/cache/apk/*;

ENV MYSQL_MAJOR 5.7
ENV MYSQL_VERSION 5.7.20-alpine3.7

EXPOSE 3306
CMD ["mysqld"]
