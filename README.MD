#docker-compose的LNMP环境
##### nginx的web目录在 www/public/，没有请手动创建
##### 常用命令
    docker-compose build
    docker-compose up
    docker-compose restart
##### 首次使用请在.env文件里配置好端口，以免与宿主机的端口冲突导致容器无法正常启动
##### 此环境包含以下容器
    nginx
    php-fpm
    mariadb
    redis
    xun-search
