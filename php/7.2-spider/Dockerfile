#name:PHP
#user:jinuary
#date:2020-05-20

FROM php:7.2
MAINTAINER jinuary 303456985@qq.com
COPY php.ini /usr/local/etc/php/conf.d/

#常用扩展
RUN apt-get update &&\
	apt-get install -y libssl-dev \
     libfreetype6-dev libjpeg62-turbo-dev libpng-dev libmemcached-dev zlib1g-dev &&\
    docker-php-ext-install bcmath gettext mysqli pdo_mysql pcntl sockets sysvsem zip &&\
    pecl install mongodb && docker-php-ext-enable mongodb && \
    docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ && \
    docker-php-ext-install -j$(nproc) gd && \
    pecl install memcached && docker-php-ext-enable memcached && \
    echo "yes\nyes\nno\nyes\nno" | pecl install swoole-4.4.16 && docker-php-ext-enable swoole && \
    echo "no\nno\nno" | pecl install redis-5.2.0 && docker-php-ext-enable redis && \
	DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends git subversion make g++ python2.7 curl chrpath wget bzip2 && \
    ln -s /usr/bin/python2.7 /usr/bin/python && \
    git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git /tmp/depot_tools && \
    export PATH="$PATH:/tmp/depot_tools" && \
    cd /usr/local/src && fetch v8 && cd v8 && \
    git checkout 5.4.500.40 && gclient sync && \
    apt install -y libncurses5 && \
	export GYPFLAGS="-Dv8_use_external_startup_data=0" && \
    export GYPFLAGS="${GYPFLAGS} -Dlinux_use_bundled_gold=0" && \
    make native library=shared snapshot=on -j4 && \
    mkdir -p /usr/local/lib && \
    cp /usr/local/src/v8/out/native/lib.target/lib*.so /usr/local/lib && \
    echo "create /usr/local/lib/libv8_libplatform.a\naddlib /usr/local/src/v8/out/native/obj.target/src/libv8_libplatform.a\nsave\nend" | ar -M && \
    cp -R /usr/local/src/v8/include /usr/local && \
    chrpath -r '$ORIGIN' /usr/local/lib/libv8.so && \
    git clone https://github.com/phpv8/v8js.git /usr/local/src/v8js && \
    cd /usr/local/src/v8js && git checkout tags/1.4.1 && phpize && ./configure --with-v8js=/usr/local && \
    export NO_INTERACTION=1 && make all -j4 && make install && \
	docker-php-ext-enable v8js && php -m && \
    \
    cd /tmp && \
    rm -rf /tmp/depot_tools /usr/local/src/v8 /usr/local/src/v8js && \
    apt-get remove -y subversion make g++ python2.7 curl libncurses5 chrpath wget bzip2 && \
#    apt-get remove -y libfreetype6-dev libjpeg62-turbo-dev libpng-dev libmemcached-dev zlib1g-dev libssl-dev && \
    apt-get autoremove -y && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*


#mcrypt
#RUN apt-get install -y  libmcrypt-dev \
#   && cd /tmp/ && curl -O http://pecl.php.net/get/mcrypt-1.0.1.tgz \
#   && tar -zxvf mcrypt-1.0.1.tgz && cd mcrypt-1.0.1 && phpize \
#   && ./configure && make && make install && docker-php-ext-enable mcrypt && rm -rf /tmp/mcrypt*
ENV PHP_OPCACHE=1 \
    PHP_DISPLAY_ERRORS=Off
#COPY index.php /var/www/html/

