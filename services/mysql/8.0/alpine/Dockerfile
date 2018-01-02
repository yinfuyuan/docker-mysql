FROM alpine:3.7

MAINTAINER yinfuyuan <yinfuyuan@gmail.com>

# add our user and group first to make sure their IDs get assigned consistently, regardless of whatever dependencies get added
RUN addgroup -S mysql && adduser -S -G mysql mysql

# grab su-exec for easy step-down from root
RUN apk add --no-cache 'su-exec>=0.2'

#RUN mkdir /docker-entrypoint-initdb.d

ENV MYSQL_MAJOR="8.0" \
    MYSQL_VERSION="8.0.2-dmr-alpine3.7" \
    MYSQL_DOWNLOAD_URL="https://cdn.mysql.com/archives/mysql-8.0/mysql-8.0.2-dmr.tar.gz" \
    MYSQL_GPG_KEY_URL="https://downloads.mysql.com/archives/gpg/?file=mysql-8.0.2-dmr.tar.gz"

RUN set -x; \
    \
	apk add --no-cache --virtual .build-deps \
	    boost-dev \
        gnupg \
        libaio-dev \
        libc-dev \
        linux-headers \
        cmake \
        g++ \
        gcc \
        libedit-dev \
        rpcgen \
        make \
        pwgen \
        perl; \
    \
    wget -O mysql-8.0.2-dmr.tar.gz "$MYSQL_DOWNLOAD_URL"; \
    wget -O mysql-8.0.2-dmr.tar.gz.asc "$MYSQL_GPG_KEY_URL"; \
    gpg --verify mysql-8.0.2-dmr.tar.gz.asc; \
    rm mysql-8.0.2-dmr.tar.gz.asc; \
    \
    mkdir -p /usr/src/mysql; \
    tar -xzf mysql-8.0.2-dmr.tar.gz -C /usr/src/mysql --strip-components=1; \
    rm mysql-8.0.2-dmr.tar.gz; \
    \
    cd /usr/src/mysql; \
    cmake . \
        -DCMAKE_INSTALL_PREFIX=/usr/local/mysql \
        -DMYSQL_DATADIR=/usr/local/mysql/data \
        -DSYSCONFDIR=/etc \
        -DEFAULT_CHARSET=utf8mb4 \
        -DDEFAULT_COLLATION=utf8mb4_general_ci \
        -DENABLED_LOCAL_INFILE=1 \
        -DEXTRA_CHARSETS=all \
        -DWITH_BOOST=/usr/local/boost_1_59_0 \
        -DDOWNLOAD_BOOST=1; \
    \
    make; \
    make install; \
    rm -r /usr/src/mysql; \
    \
    apk del .build-deps

EXPOSE 3306
CMD ["mysqld"]
