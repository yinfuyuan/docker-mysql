FROM alpine:3.7

MAINTAINER yinfuyuan <yinfuyuan@gmail.com>

# add our user and group first to make sure their IDs get assigned consistently, regardless of whatever dependencies get added
RUN addgroup -S mysql && adduser -S -G mysql mysql

# grab su-exec for easy step-down from root
RUN apk add --no-cache 'su-exec>=0.2'

#RUN mkdir /docker-entrypoint-initdb.d

ENV MYSQL_MAJOR="8.0" \
    MYSQL_VERSION="8.0.2-dmr"

ENV MYSQL_DOWNLOAD_URL="http://cdn.mysql.com/Downloads/MySQL-${MYSQL_MAJOR}/mysql-${MYSQL_VERSION}.tar.gz" \
    MYSQL_GPG_KEY_URL="http://cdn.mysql.com/Downloads/MySQL-${MYSQL_MAJOR}/mysql-${MYSQL_VERSION}.tar.gz.asc" \
    GPG_KEY="A4A9406876FCBD3C456770C88C718D3B5072E1F5"

RUN set -x; \
    \
	apk add --no-cache --virtual .build-deps \
        cmake \
        curl \
        g++ \
        gcc \
        gnupg \
        libaio-dev \
        libc-dev \
        libedit-dev \
        linux-headers \
        make \
        perl \
        rpcgen \
        portablexdr-dev \
        ncurses \
        pwgen; \
    \
    wget -O mysql-${MYSQL_VERSION}.tar.gz "${MYSQL_DOWNLOAD_URL}"; \
    wget -O mysql-${MYSQL_VERSION}.tar.gz.asc "${MYSQL_GPG_KEY_URL}"; \
    gpg --keyserver ha.pool.sks-keyservers.net --recv-keys "${GPG_KEY}"; \
    gpg --verify mysql-${MYSQL_VERSION}.tar.gz.asc; \
    rm mysql-${MYSQL_VERSION}.tar.gz.asc; \
    \
    mkdir -p /usr/src/mysql; \
    tar -xzf mysql-${MYSQL_VERSION}.tar.gz -C /usr/src; \
    rm mysql-${MYSQL_VERSION}.tar.gz; \
    \
    cd /usr/src/mysql-${MYSQL_VERSION}; \
    cmake . -DBUILD_CONFIG=mysql_release \
        -DWITH_BOOST=/usr/local \
        -DDOWNLOAD_BOOST=1; \
    \
    make; \
    make install; \
    rm -r /usr/src/mysql; \
#    \
#    apk del .build-deps

#EXPOSE 3306
#CMD ["mysqld"]
