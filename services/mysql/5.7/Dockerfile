FROM alpine:3.7

MAINTAINER yinfuyuan <yinfuyuan@gmail.com>

RUN addgroup -S mysql && adduser -S -G mysql mysql

# grab su-exec for easy step-down from root
RUN apk add --no-cache 'su-exec>=0.2'

ENV MYSQL_MAJOR 5.7
ENV MYSQL_VERSION 3.2.11-alpine3.7
ENV MYSQL_DOWNLOAD_URL https://cdn.mysql.com//Downloads/MySQL-5.7/mysql-5.7.20.tar.gz
ENV MYSQL_DOWNLOAD_MD5 362aaec9a5d88567e5e18d929caa319e

RUN set -ex; \
	\
	apk add --no-cache --virtual .build-deps \
		coreutils \
		gcc \
		linux-headers \
		make \
		musl-dev \
	; \
	wget -O mysql.tar.gz "$MYSQL_DOWNLOAD_URL"; \
	echo "$MYSQL_DOWNLOAD_MD5 *mysql.tar.gz" | md5sum -c -; \
	mkdir -p /usr/src/mysql; \
	tar -xzf mysql.tar.gz -C /usr/src/mysql --strip-components=1; \
	rm mysql.tar.gz; \
	apk del .build-deps



RUN mkdir /docker-entrypoint-initdb.d

#RUN apk update; \
#    apk add openrc pwgen openssl; \
#    rm -rf /tmp/* /var/cache/apk/*;

EXPOSE 3306
#CMD ["mysqld"]
