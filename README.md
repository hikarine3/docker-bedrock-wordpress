# About other language's document (他の言語での説明について)
日本語の説明は　README-ja.md　の方に書いてあるので、そちらを参照して下さい。

# What is this repository

This repository is for initiating your WordPress development environment with best architecture.

## Features (English)
- WordPress's core, plugin and themes can be updated through PHP's composer. You can control all of compontents through composer.json, which means that you can reproduce your WordPress's environment easily.
- Docker is supported. So you can bring up your development environment easily just typing `docker-compose up`.
- Multiple WordPress's environment can be easily created with simple configuration based on domainname or top directory.
- Even for production environment if you are not using docker, you can easily have multiple WordPress environment with 1 source code on 1 instance using some trick for .env and upload directory. For example, if you want to use WordPress for vpsranking.com, you just have to prepare
.env.vpsranking.com
- Plugins which must be enabled for all environments are added in composer.json from the beginning

You can use this for initiation of all your WordPress's projects.  
If you have any questions, please ask to this repository's admin [@hajimekurita](https://twitter.com/hajimekurita) through twitter DM after following its account.

# Used software
- Docker https://www.docker.com/products/docker-desktop
- docker-compose https://docs.docker.com/compose/install/
- MariaDB docker: https://hub.docker.com/_/mariadb
- Apache 2.4 & PHP 8.2 docker: https://hub.docker.com/layers/library/php/8.2-apache/images/sha256-89ad17cca246e8a6ce742b5b89ce65b34ce6223204a282e45f72b4f758ff6401?context=explore
- WordPress: https://wordpress.org/download/
- Bedrock: https://roots.io/bedrock/ 2023/09/28 ver https://github.com/roots/bedrock/commit/a90180a1a0c3b57f7ed561ddb65aca46de839c86

# How to Install
```
# Download github repository
git clone git@github.com:hikarine3/docker-bedrock-wordpress.git;
cd docker-bedrock-wordpress;
cd bedrock;

# Install PHP modules including WordPress Core, Themes adn plugins#
composer.phar update;

# Create unique salt for security of your WordPress#
wp dotenv salts regenerate;
```

# Make development environment up through docker
First you have to install docker and make the process run.

Then under the directory of docker-bedrock-wordpress which has docker-composer.yml,

```
docker-compose up -d;
```

Then type following commands by copy & paste.
```
docker exec -i -t `docker ps|grep php|awk '{print $1}'`  docker-php-ext-install mysqli;
docker exec -i -t `docker ps|grep php|awk '{print $1}'` /bin/bash -c 'cp /usr/local/etc/php/php.ini-production /usr/local/etc/php/php.ini';
docker exec -i -t `docker ps|grep php|awk '{print $1}'` /bin/bash -c 'echo extension=mysqli >>  /usr/local/etc/php/php.ini';
docker exec -i -t `docker ps|grep php|awk '{print $1}'` /bin/bash -c 'echo log_errors=On >>  /usr/local/etc/php/php.ini';
docker exec -i -t `docker ps|grep php|awk '{print $1}'` /bin/bash -c 'echo error_reporting=E_ALL >>  /usr/local/etc/php/php.ini';

docker exec -i -t `docker ps|grep php|awk '{print $1}'` /bin/bash -c  'sed -i "/<Directory \/var\/www\/>/,/<\/Directory>/ s/AllowOverride None/AllowOverride all/" /etc/apache2/apache2.conf';
docker exec -i -t `docker ps|grep php|awk '{print $1}'` /bin/bash -c  'a2enmod rewrite'
docker exec -i -t `docker ps|grep php|awk '{print $1}'` /usr/sbin/apachectl restart;
```

If you want to see error message, then you can do by 
```
docker logs -f `docker ps|grep php|awk '{print $1}'`
```

Now you can see a web site by seeing http://localhost/

If you want to stop docker's environment, you can stop it by typing

```
docker-compose down;
```

If you didn't edit docker-compose.yml, then you can login MariaDB by typing

```
docker exec -i -t `docker ps|grep mariadb|awk '{print $1}'` /bin/bash
```

through docker.

If you have changed user and password before you first make docker up, then change user and password.

# Add wordpress plugin and themes

Edit bedrock/composer.json referencing to 

Plugins

https://wpackagist.org/ ( Package must be reflected to https://wpackagist.org/ , which has some timelag)


Themes

https://wordpress.org/themes/ 

After editing composer.json, type `composer.phar update`.

composer.json
```
  "require": {
    "php": ">=8.0",
    "composer/installers": "^2.2",
    "vlucas/phpdotenv": "^5.5",
    "oscarotero/env": "^2.1",
    "roots/bedrock-autoloader": "^1.0",
    "roots/bedrock-disallow-indexing": "^2.0",
    "roots/wordpress": "6.3.1",
    "roots/wp-config": "1.0.0",
    "roots/wp-password-bcrypt": "1.1.0",
    "wpackagist-theme/twentytwentythree": "^1.0",
    "wpackagist-plugin/multilingual-press": "*"
  },
```

It is important for you not to install anything through WordPress's admin screen if you want to controll WordPress, themes and plugins through souce code.

By it, you can make the version down if necessary easily through command line even if admin screen becomes blank by updated WP plugins.



# Create multiple wordpress environments
## /u$Number/　


Create .env.u$Number by copying 

```
cp bedrock/.env bedrock/.env.u$Number;
```

In this code,
.env.u1
is already ready as an example.

Login to database and create addtional database.

If the content of env file is like this without changing value of docker-compose.yml,
```
DB_NAME=u1
DB_USER=${WORDPRESS_DB_USER}
DB_PASSWORD=${WORDPRESS_DB_PASSWORD}
```

Then you can log in DB by typing
```
docker exec -i -t `docker ps|grep mariadb|awk '{print $1}'` mariadb --user=root --password=ExampleRootPass 
```

Then
```
CREATE DATABASE u1 character set utf8mb4;
GRANT ALL PRIVILEGES ON u1.* TO'wpdbuser'@'%';
```

After it,
```
docker exec -i -t `docker ps|grep php|awk '{print $1}'` /bin/bash -c  'mkdir -p /var/www/web;cd /var/www/web;ln -s ../web u1';
```

http://localhost/u1/

is now available

## $DOMAINA
Create file .env.$Domain by
```
cp bedrock/.env .env.$Domain
```

In this code,
.env.example.com
is ready as example.



If the content of env file is like this without changing value of docker-compose.yml,
```
DB_NAME=example
DB_USER=${WORDPRESS_DB_USER}
DB_PASSWORD=${WORDPRESS_DB_PASSWORD}
WP_HOME=http://example.com
```

Then you can log in DB by typing
```
docker exec -i -t `docker ps|grep mariadb|awk '{print $1}'` mariadb --user=root --password=ExampleRootPass 
```


```
CREATE DATABASE example character set utf8mb4;
GRANT ALL PRIVILEGES ON example.* TO'wpdbuser'@'%';
```

Now you can see Wordpress screen at

http://example.com/



# Reference: Change to original bedrock

```
cd bedrock;
rm -rf html;
ln -s web html;

ln -s ../composer.phar composer.phar
```

Modified berock/config/application.php

# License

MIT

# Author

## Name
Hajime Kurita

## Twitter
- EN: https://twitter.com/hajimekurita
- JP: https://twitter.com/hikarine3
- CN: https://www.weibo.com/u/7334273967

## Technical web services which can help you
### VPS & Infra
- EN: https://vpsranking.com/
- JP: https://vpshikaku.com/
- CN: https://vpsranking.com/zh/

### Programming
- EN: https://programminglang.com/en/
- JP: https://programminglang.com/ja/

### Source code repositories
- Github https://github.com/hikarine3
- Docker https://hub.docker.com/u/1stclass/
- NPM https://www.npmjs.com/~hikarine3
- Perl http://search.cpan.org/~hikarine/
- PHP https://packagist.org/packages/hikarine3/
- Python https://pypi.org/user/hikarine3/

### Corporation page & services
- EN: https://1stclass.co.jp/en/services-en/
- JP: https://1stclass.co.jp/services/
