# sd-swoole
FROM swoole:latest
MAINTAINER Jincheng Zhang 896369042@qq.com
RUN mkdir /apps
COPY composer.json /apps/composer.json
# 安装SD框架
RUN cd /apps && composer config -g repo.packagist composer https://packagist.laravel-china.org && composer install && php vendor/tmtbe/swooledistributed/src/Install.php -y

CMD ["php","/apps/bin/start_swoole_server.php","start"]