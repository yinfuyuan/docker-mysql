FROM alpine:3.7

MAINTAINER yinfuyuan <yinfuyuan@gmail.com>

# add our user and group first to make sure their IDs get assigned consistently, regardless of whatever dependencies get added
RUN addgroup -S mysql && adduser -S -G mysql mysql

# grab su-exec for easy step-down from root
RUN apk add --no-cache 'su-exec>=0.2'

RUN mkdir /docker-entrypoint-initdb.d

RUN set -x; \
	apk add --no-cache --virtual .build-deps \
        cmake \
        g++ \
        gcc \
        libedit-dev \
        make \
        pwgen \
        openssl \
        perl

ENV MYSQL_MAJOR 8.0
ENV MYSQL_VERSION 8.0.2-dmr-alpine3.7
ENV MYSQL_DOWNLOAD_URL https://cdn.mysql.com/archives/mysql-8.0/mysql-8.0.2-dmr.tar.gz
ENV MYSQL_GPG_KEY_URL https://downloads.mysql.com/archives/gpg/?file=mysql-8.0.2-dmr.tar.gz

RUN set -x; \
    wget -O mysql-8.0.2-dmr.tar.gz "$MYSQL_DOWNLOAD_URL"; \
    wget -O mysql-8.0.2-dmr.tar.gz.asc "$MYSQL_GPG_KEY_URL"; \
    gpg --verify mysql-8.0.2-dmr.tar.gz.asc; \
    rm mysql-8.0.2-dmr.tar.gz.asc; \
    mkdir -p /usr/src/mysql; \
    tar -xzf mysql-8.0.2-dmr.tar.gz -C /usr/src/mysql --strip-components=1; \
    rm mysql-8.0.2-dmr.tar.gz; \
    cd /usr/src/mysql; \
    cmake . \
        -DCMAKE_INSTALL_PREFIX=/usr/local/mysql \
        -DMYSQL_DATADIR=/usr/local/mysql/data \
        -DWITH_BOOST=/usr/local/boost_1_59_0 \
        -DSYSCONFDIR=/etc \
        -DEFAULT_CHARSET=utf8mb4 \
        -DDEFAULT_COLLATION=utf8mb4_general_ci \
        -DENABLED_LOCAL_INFILE=1 \
        -DEXTRA_CHARSETS=all \
        -DDOWNLOAD_BOOST=1; \
    make; \
    make install; \
    rm -r /usr/src/mysql; \

RUN set -x; \
    apk del .build-deps; \
    rm -rf /tmp/* /var/cache/apk/*

EXPOSE 3306
CMD ["mysqld"]
