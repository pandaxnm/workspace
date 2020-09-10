FROM php:7.4-fpm
LABEL maintainer="pandaxnm"

# set timezome
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

COPY ./sources.list /etc/apt/sources.list

COPY bluecity-qconf_1.2.3-1_amd64.deb bluecity-qconf_1.2.3-1_amd64.deb
# 安装qconf
RUN dpkg -i bluecity-qconf_1.2.3-1_amd64.deb

# Install package and PHP Core extensions
RUN apt-get update && apt-get upgrade -y \
    && apt-get install -y \
    g++ \
    libbz2-dev \
    libc-client-dev \
    libcurl4-gnutls-dev \
    libedit-dev \
    libfreetype6-dev \
    libicu-dev \
    libjpeg62-turbo-dev \
    libkrb5-dev \
    libldap2-dev \
    libmagickwand-dev \
    libmcrypt-dev \
    libmemcached-dev \
    libpq-dev \
    libsqlite3-dev \
    libssl-dev \
    libreadline-dev \
    libxslt1-dev \
    memcached \
    unzip \
    wget \
    zlib1g-dev \
    git \
    cmake \
    automake \
    autoconf \
    unzip \
    curl \
    libzip-dev \
    m4 \
    perl \
    libtool \
    libtool-bin \
#    texinfo \
#    && cd /tmp \
#    && wget https://ftp.gnu.org/gnu/automake/automake-1.14.tar.gz \
#    && tar -xzvf automake-1.14.tar.gz \
#    && cd automake-1.14 \
#    && ./configure \
#    && make && make install \
#    && cd /tmp \
#    && wget https://ftp.gnu.org/gnu/automake/automake-1.11.tar.gz \
#    && tar -xzvf automake-1.11.tar.gz \
#    && cd automake-1.11 \
#    && ./configure \
#    && make && make install \
#    # 安装 QConf
#    && git clone https://github.com/Qihoo360/QConf.git \
#    && cd QConf \
#    && mkdir build && cd build \
#    && cmake .. \
#    && make \
#    && make install \
#    && cd ../driver/php \
#    && phpize \
#    && ./configure \
#    && make \
#    && make install \
#    && docker-php-ext-enable qconf \
    # 安装 swoole
    && cd /tmp \
    && wget https://github.com/swoole/swoole-src/archive/master.tar.gz \
    && tar zxvf master.tar.gz \
    && cd swoole-src-master \
    && phpize \
    && ./configure \
    --enable-openssl \
    --enable-http2 \
    && make \
    && make install \
    && docker-php-ext-enable swoole \
    && cd /tmp \
    # 安装 rdkafka
    && git clone https://github.com/edenhill/librdkafka.git \
    && cd librdkafka \
    && ./configure \
    && make \
    && make install \
    && cd /tmp \
    # 安装 amqp
    && git clone https://github.com/alanxz/rabbitmq-c \
    && cd rabbitmq-c \
    && mkdir build && cd build \
    && cmake .. \
    && make \
    && make install \
    && cd /tmp \
    # 安装 ffi
    && wget https://github.com/libffi/libffi/releases/download/v3.3/libffi-3.3.tar.gz \
    && tar zxvf libffi-3.3.tar.gz \
    && cd libffi-3.3 \
    && ./configure \
    && make && make install \
    && docker-php-ext-configure gd \
    --with-freetype=/usr/include/ \
    --with-jpeg=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-install -j$(nproc) ffi \
    && docker-php-ext-configure imap --with-kerberos --with-imap-ssl \
    && docker-php-ext-install -j$(nproc) imap \
    && docker-php-ext-configure intl \
    && docker-php-ext-install -j$(nproc) intl \
    && docker-php-ext-configure ldap --with-libdir=lib/x86_64-linux-gnu/ \
    && docker-php-ext-install ldap \
    && docker-php-ext-install -j$(nproc) \
    bcmath \
    bz2 \
    calendar \
    exif \
    gettext \
    mysqli \
    opcache \
    pdo_mysql \
    pdo_pgsql \
    pgsql \
    soap \
    sockets \
    xmlrpc \
    xsl \
    zip \
    && pecl install xdebug && docker-php-ext-enable xdebug \
    && pecl install redis && docker-php-ext-enable redis \
    && pecl install rdkafka && docker-php-ext-enable rdkafka \
    && pecl install amqp && docker-php-ext-enable amqp \
    && pecl install apcu && docker-php-ext-enable apcu \
    && pecl install uuid && docker-php-ext-enable uuid \
    && pecl install msgpack && docker-php-ext-enable msgpack \
    && yes '' | pecl install imagick && docker-php-ext-enable imagick \
    && docker-php-source delete \
    && apt-get remove -y g++ wget \
    && apt-get autoremove --purge -y && apt-get autoclean -y && apt-get clean -y \
    && rm -rf /var/lib/apt/lists/* \
    && rm -rf /tmp/* /var/tmp/*

# Install Composer
ENV COMPOSER_HOME /root/composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
ENV PATH $COMPOSER_HOME/vendor/bin:$PATH

WORKDIR /data

# Write Permission
RUN usermod -u 1000 www-data
