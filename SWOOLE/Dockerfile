# sd-swoole
FROM php:7.2-cli-stretch
MAINTAINER Jincheng Zhang 896369042@qq.com
# 构建swoole环境，在这里安装了php,swoole,composer
RUN mv /etc/apt/sources.list.d /etc/apt/sources.list.d.bak \
	&& mv /etc/apt/sources.list /etc/apt/sources.list.bak \
	&& echo deb http://mirrors.aliyun.com/debian stretch main contrib non-free > /etc/apt/sources.list \
	&& echo deb http://mirrors.aliyun.com/debian stretch-proposed-updates main contrib non-free >> /etc/apt/sources.list \
	&& echo deb http://mirrors.aliyun.com/debian stretch-updates main contrib non-free >> /etc/apt/sources.list \
	&& echo deb http://mirrors.aliyun.com/debian-security/ stretch/updates main non-free contrib >> /etc/apt/sources.list \
	&& apt-get update && apt-get install -y \
	zlib1g-dev \
	vim \
	libssl-dev \
	unzip \
	wget \
	git \
	make \
	supervisor \
	--no-install-recommends \
	&& docker-php-ext-install zip opcache bcmath pdo_mysql \
	&& cd /home && rm -rf temp && mkdir temp && cd temp \
	#&& wget https://github.com/swoole/swoole-src/archive/v1.9.19.tar.gz \
	wget https://github.com/redis/hiredis/archive/v0.13.3.tar.gz https://github.com/phpredis/phpredis/archive/3.1.3.tar.gz \
	&& tar -xzvf 3.1.3.tar.gz \
	&& tar -xzvf v0.13.3.tar.gz \
	#&& tar -xzvf v1.9.19.tar.gz \
	&& cd /home/temp/hiredis-0.13.3 \
	&& make -j && make install && ldconfig \
	#&& cd /home/temp/swoole-src-1.9.19 \
	#&& phpize && ./configure --enable-async-redis --enable-openssl && make \
	#&& make install \
	&& pecl install inotify \
	&& pecl install ds \
	&& pecl install igbinary \
	&& pecl install swoole \
	&& cd /home/temp/phpredis-3.1.3 \
	&& phpize \
	&& ./configure --enable-redis-igbinary \
	&& make &&  make install \
	&& cd /home/temp \
	&& php -r"copy('https://getcomposer.org/installer','composer-setup.php');" \
	&& php composer-setup.php --install-dir=/usr/bin --filename=composer \
	&& rm -rf /home/temp \
	&& cd /usr/local/etc/php/conf.d/ \
	&& echo extension=igbinary.so>igbinary.ini \
	&& echo extension=redis.so>redis.ini \
	&& echo extension=inotify.so>inotify.ini \
	&& echo extension=swoole.so>swoole.ini \
	&& echo extension=ds.so>ds.ini \
	&& composer config -g repo.packagist composer https://packagist.laravel-china.org \
	&& mkdir -p /var/log/supervisor \
	&& apt-get autoclean \
    && apt-get autoremove \
    && rm -rf /var/lib/apt/lists/*

COPY ./config/* /usr/local/etc/php/conf.d/

CMD ["bin/bash"]