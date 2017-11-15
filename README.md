## Sample docker-compose configuration for this image
```yaml
version: "3.1"
services:
    db:
      image: mariadb:10.1
      environment:
        - MYSQL_ROOT_PASSWORD=pass
        - MYSQL_DATABASE=db
        - MYSQL_USER=db
        - MYSQL_PASSWORD=db
      volumes:
      # synchornize db files
          - ./db/mysql:/var/lib/mysql
      ports:
      # set private ip (for multiple docker container)
        - 127.0.0.10:3306:3306
    php:
        container_name: php
        image: dzordz/xdebug-nginx-php
        ports:
            - 127.0.0.10:80:80
        environment:
        # xdebug cli config
            - XDEBUG_CONFIG=remote_host=192.168.2.127 idekey=PHPSTORM
            - PHP_IDE_CONFIG=serverName=docker.loc
        volumes:
            # sync main directory
            - ../:/var/www/app
            # sync other vhost
            - ./vhost/default.conf:/etc/nginx/sites-available/default.conf
            # add ext php confi
            - ./php/php.ini:/usr/local/etc/php/conf.d/php_extra.ini
            # sync bash_history
            - ./.bash_history:/root/.bash_history
```