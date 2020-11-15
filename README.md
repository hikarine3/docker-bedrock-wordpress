# What is this repository (このレポジトリは何ですか)
This repository is for initiating your WordPress development environment with best architecture.

## Features (English)
- WordPress's core, plugin and themes can be updated through PHP's composer. You can control all of compontents through composer.json, which means that you can reproduce your WordPress's environment easily.
- Docker is supported. So you can bring up your development environment easily just typing `docker-compose up`.
- Multiple WordPress's environment can be easily created just by increasing definition of WordPress's environment defined in docker-compose.yml
- Even for production environment if you are not using docker, you can easily have multiple WordPress environment with 1 source code on 1 instance using some trick for .env and upload directory. For example, if you want to use WordPress for vpsranking.com, you just have to prepare
.env.vpsranking.com
- Plugins which must be enabled for all environments are added in composer.json from the beginning

You can use this for initiation of all your WordPress's projects.  
If you have any questions, please ask to this repository's admin [@hajimekurita](https://twittter.com/hajimekurita) through twitter DM after following its account.

## 特徴 (日本語)

このレポジトリはWordPressで将来の運用を見越した開発を行うにあたっての、ベストな構成を提供するWordPress開発テンプレートとなっています。
- WordPress本体、プラグイン、テーマ全てがPHPのcomposerでインストール・更新できます。composer.jsonの定義に従ってインストールされるので、構成をGit等でバージョン管理し、環境再現が簡単になります。
- Dockerがサポートされており、必要なら簡単にDockerでWordPressを立ち上げ、開発する事が出来ます。
- 複数のWordPressの環境を、同一ソースコード・同一サーバー上に、簡単に立ち上げる事が出来ます。Docker環境ではdocker-compose.ymlを、Dockerを使わない環境では
.env.Webホスト名
例: vpshikaku.com の場合には
.env.vpshikaku.com
を触るだけで済みます。
- どんな環境でもインストール・有効化するに値するPluginが最初からcomposer.jsonに定義されています

このWordPressのインストール方法は「[WordPressの利用法/設定/有用プラグイン/テーマ/開発法まとめ](https://vpshikaku.com/wordpress/)」で紹介されてる方法の内、上級者向けの方法になります。  
上級者向け(=エンジニア向け)ですが、使いこなせれば、相当便利な手法になります。
このページは「英語 => 日本語」と、同じ内容が別言語で説明されてる構成になっています。  
ご質問がありましたら、このレポジトリの管理者 [@hikarine](https://twitter.com/hikarine3) にTwitterでフォローしてDMでお問合せ下さい。

# Used software (使われているソフト)
- Docker https://www.docker.com/products/docker-desktop
- docker-compose https://docs.docker.com/compose/install/
- MariaDB docker: https://hub.docker.com/_/mariadb
- Apache 2.4 & PHP 7.3 docker: https://hub.docker.com/r/1stclass/docker-apache24-php7
- WordPress: https://wordpress.org/download/
- Bedrock: https://roots.io/bedrock/

Web sever must allow symbolic links. (Webサーバーはシンボリックリンクを有効にしておく必要があります)

# How to Install (インストール手順)
```
# Download github repository ( githubのレポジトリーをダウンロード ) #
git clone git@github.com:hikarine3/docker-bedrock-wordpress.git;
cd docker-bedrock-wordpress;
cd bedrock;

# Install PHP modules including WordPress Core, Themes adn plugins (WPのファイル群をインストール) #
composer.phar update;

# Create unique salt for security of your WordPress (Saltをユニークに更新してセキュリティ向上) #
wp dotenv salts regenerate;
```

# Make development environment up through docker
First you have to install docker and make the process run.

Then under the directory of docker-bedrock-wordpress which has docker-composer.yml,

```
docker-compose up;
```

If you add -d, then you make the process run as daemon which will continue to run even after you close terminal

Then after some time, you can see brought up WordPress's setting up screen at
http://localhost:10080/

If you want to stop docker's environment, you can stop it by typing

```
docker-compose down;
```

If you didn't edit docker-compose.yml, then you can login MariaDB by typing

```
docker exec -it local_mariadb mysql --host=local_mariadb --user=root --password=ExampleRootPass
```

through docker.

If you have changed user and password before you first make docker up, then change user and password.

# 開発環境の立ち上げ
まず貴方はDockerのインストールをして、それを走らせておく必要があります。

その上で、docker-bedrock-wordpressディレクトリの中=docker-composer.ymlが置いてあるディレクトリで、

```
docker-compose up;
```

と打ってください。

尚「 -d」をつけて打つとターミナルを閉じてもプロセスは走り続けます。

暫く経ったら、

http://localhost:10080/

でWordPressの設定画面を見る事が出来ます。

MariaDBには、docker-compose.ymlがデフォルトの設定のままなら
```
docker exec -it local_mariadb mysql --host=local_mariadb --user=root --password=ExampleRootPass
```
でdocker経由だとログインできます。

Docker環境を立ち上げる前に、ユーザーとパスワードを変更してたら、それに合わせて変更して下さい。

docker環境を止めたければ

```
docker-compose down;
```

と叩いて下さい。


後はローカルPCでWordPressのコンテンツ整備をしたら

ソースコードとしてはこのレポジトリの中身をリモートのサーバーに配布し

DBの中身は

https://wordpress.org/plugins/all-in-one-wp-migration/

といったプラグインを使って、ローカルPCからリモートサーバーに移動させれば、リモートでも動かす事が出来ます。

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
    "composer/installers": "*",
    "johnpbloch/wordpress": "*",
    "oscarotero/env": "*",
    "php": ">=7.1",
    "roots/wp-password-bcrypt": "*",
    "vlucas/phpdotenv": "2.5.1",

    "wpackagist-plugin/disable-author-pages": "*",
    "wpackagist-plugin/disable-comments": "*",
    "wpackagist-plugin/duplicate-post": "*",

    "wpackagist-theme/astra":"*",
    "wpackagist-theme/online-shop":"*"
  },
```

It is important for you not to install anything through WordPress's admin screen if you want to controll WordPress, themes and plugins through souce code.

By it, you can make the version down if necessary easily through command line even if admin screen becomes blank by updated WP plugins.

# WordPress Pluginやテーマの追加
bedrock/composer.jsonを 

プラグイン

https://wordpress.org/plugins/ ( https://wpackagist.org/ に反映されてから使えるようになる/少々タイムラグ有り)

や 

テーマ

https://wordpress.org/themes/

を参照しながら編集して、編集が終わったら`composer.phar update`を叩いて下さい。

composer.json
```
  "require": {
    "composer/installers": "*",
    "johnpbloch/wordpress": "*",
    "oscarotero/env": "*",
    "php": ">=7.1",
    "roots/wp-password-bcrypt": "*",
    "vlucas/phpdotenv": "2.5.1",

    "wpackagist-plugin/disable-author-pages": "*",
    "wpackagist-plugin/disable-comments": "*",
    "wpackagist-plugin/duplicate-post": "*",

    "wpackagist-theme/astra":"*",
    "wpackagist-theme/online-shop":"*"
  },
```


管理画面から追加せず、あくまでcomposer.jsonの編集とcomposer updateだけで管理するのが、ソースコードでWordPressの構成管理を行い切るコツです。

これにより、WordPressのプラグインのインストールで真っ白になってしまったとしても、簡単にcomposer.jsonを編集してcomposer updateするだけで、問題のプラグインを元に戻す事も出来ますし、問題の解決もより早く行えます。

# Add more WordPress environment through Docker

```
vi docker-compose.yml
```

Add more pattern form WordPress
```
  apache2:
    depends_on:
      - db
    image: 1stclass/docker-apache24-php7
    container_name: local_web1
    ports:
      - "10081:80"
    restart: always
    environment:
      APACHE_DOCUMENT_ROOT: /var/www/web
      WORDPRESS_DB_PREFIX: bdr
      WORDPRESS_DATABASE: wordpress2
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: wpdbuser
      WORDPRESS_DB_PASSWORD: wpdbuserpass
    volumes:
      - type: bind
        source: ./bedrock
        target: /var/www
```

Then log in to mysql using root password and create new database and git access privilege to defined wordpress's user.

```
GRANT ALL PRIVILEGES ON wordpress2.* TO'wpdbuser'@'localhost';
GRANT ALL PRIVILEGES ON wordpress2.* TO'wpdbuser'@'10.%';
GRANT ALL PRIVILEGES ON wordpress2.* TO'wpdbuser'@'172.%';
GRANT ALL PRIVILEGES ON wordpress2.* TO'wpdbuser'@'192.%';
```

After it, if you restart docker-compose environment, you shold be able to access to another WordPress environment with defined added Port.

Stop docker
```
docker-compose down
```

Start again
```
docker-compose up
```

# 更なるWordPress環境のDockerを通じた追加

```
vi docker-compose.yml
```

追加のWordPress環境の定義を追加します。
```
  apache2:
    depends_on:
      - db
    image: 1stclass/docker-apache24-php7
    container_name: local_web1
    ports:
      - "10081:80"
    restart: always
    environment:
      APACHE_DOCUMENT_ROOT: /var/www/web
      WORDPRESS_DB_PREFIX: bdr
      WORDPRESS_DATABASE: wordpress2
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: wpdbuser
      WORDPRESS_DB_PASSWORD: wpdbuserpass
    volumes:
      - type: bind
        source: ./bedrock
        target: /var/www
```

MariaDBにrootユーザーでログインして、docker-compose.ymlに定義した追加データベースを作成します。

Docker経由でのアクセスは
```
docker exec -it local_mariadb mysql --host=local_mariadb --user=root --password=ExampleRootPass
```

```
CREATE DATABASE wordpress2 character set utf8mb4;
```

また、ユーザーに作ったデータベースへのアクセスけんを付与します。
```
GRANT ALL PRIVILEGES ON wordpress2.* TO'wpdbuser'@'localhost';
GRANT ALL PRIVILEGES ON wordpress2.* TO'wpdbuser'@'10.%';
GRANT ALL PRIVILEGES ON wordpress2.* TO'wpdbuser'@'172.%';
GRANT ALL PRIVILEGES ON wordpress2.* TO'wpdbuser'@'192.%';
```

それが終わったら

```
docker-compose down
```

で一旦docker環境を止めて

```
docker-compose up
```

で再起動して下さい。

上記の例でしたら

http://localhost:10081/

が新たに使えるようになります。

# 

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
