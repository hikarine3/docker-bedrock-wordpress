# What is this repository (このレポジトリは何ですか)
This repository is for initiating your WordPress development environment with best architecture.

## Features (English)
- WordPress's core, plugin and themes can be updated through PHP's composer. You can control all of compontents through composer.json, which means that you can reproduce your WordPress's environment easily.
- Docker is supported. So you can bring up your development environment easily just typing `docker-compose up`.
- Multiple WordPress's environment can be easily created just by increasing definition of WordPress's environment defined in docker-compose.yml
- Even for production environment if you are not using docker, you can easily have multiple WordPress environment with 1 source code on 1 instance using some trick for .env and upload directory 

You can use this for initiation of all your WordPress's projects.

## 特徴 (日本語)

このレポジトリはWordPressで将来の運用を見越した開発を行うにあたっての、ベストな構成を提供するWordPress開発テンプレートとなっています。
- WordPress本体、プラグイン、テーマ全てがPHPのcomposerでインストール・更新できます。composer.jsonの定義に従ってインストールされるので、構成をGit等でバージョン管理し、環境再現が簡単になります。
- Dockerがサポートされており、必要なら簡単にDockerでWordPressを立ち上げ、開発する事が出来ます。
- 複数のWordPressの環境を、同一ソースコード・同一サーバー上に、簡単に立ち上げる事が出来ます。Docker環境ではdocker-compose.ymlを、Dockerを使わない環境では.env.Webホスト名を触るだけで済みます。

# Used software (使われているソフト)
- Docker
- docker-compose
- MariaDB: latest
- PHP 7.3
- Wordpress
- Bedrock

# How to Install (インストール手順)
```
# Download github repository
git clone git@github.com:hikarine3/docker-bedrock-wordpress.git;
cd docker-bedrock-wordpress;
cd bedrock;

# Install PHP modules including WordPress Core, Themes adn plugins;
composer.phar update;

# Create unique salt for security of your WordPress
wp dotenv salts regenerate;
```

# Make development environment up through docker

In the environment where you are making Docker running,

```
cd docker-bedrock-wordpress;
docker-compose up -d;
```

If you want to stop docker's environment, you can stop it by typing

```
docker-compose down;
```

Then after some time, you can see brought up WordPress's setting up screen at
http://localhost/


If you didn't edit docker-compose.yml, then you can login MariaDB by typing

```
mysql --host=127.0.0.1 --user=root --password=somewordpress
```

If you have changed user and password before you first make docker up, then change user and password.

# 開発環境の立ち上げ

Dockerが立ち上がってる環境で
```
cd docker-bedrock-wordpress;
docker-compose up -d;
```

と打ってください。

暫く経ったら、

http://localhost/

でWordPressの設定画面を見る事が出来ます。

MariaDBには、docker-compose.ymlがデフォルトの設定のままなら
```
mysql --host=127.0.0.1 --user=root --password=somewordpress
```
でログインできます。

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


# Add wordpress plugin (Wordpress Pluginの追加)

Edit bedrock/composer.json referencing to https://wpackagist.org/ and after editing, type composer.phar update.

It is important for you not to install anything through WordPress's admin screen if you want to controll WordPress, themes and plugins through souce code.

By it, you can make the version down if necessary easily through command line even if admin screen becomes blank by updated WP plugins.

# Wordpress Pluginの追加
bedrock/composer.jsonを https://wpackagist.org/ を参照しながら編集して、編集が終わったらcomposer.phar updateを叩いて下さい。

管理画面から追加せず、あくまでcomposer.jsonの編集とcomposer updateだけで管理するのが、ソースコードでWordPressの構成管理を行い切るコツです。

これにより、WordPressのプラグインのインストールで真っ白になってしまったとしても、簡単にcomposer.jsonを編集してcomposer updateするだけで、問題のプラグインを元に戻す事も出来ますし、問題の解決もより早く行えます。

# License

MIT

# Author

## Name
Hajime Kurita

## Twitter
- EN: https://twitter.com/hajimekurita
- JP: https://twitter.com/hikarine3
- CN: https://www.weibo.com/u/7334273967

# Technical web services
## VPS & Infra
- EN: https://vpsranking.com/
- JP: https://vpshikaku.com/
- CN: https://vpsranking.com/zh/

## Programming
- EN: https://programminglang.com/en/
- JP: https://programminglang.com/ja/

## Source code repositories
- Github https://github.com/hikarine3
- Docker https://hub.docker.com/u/1stclass/
- NPM https://www.npmjs.com/~hikarine3
- Perl http://search.cpan.org/~hikarine/
- PHP https://packagist.org/packages/hikarine3/
- Python https://pypi.org/user/hikarine3/

# Corporation page & services
- EN: https://1stclass.co.jp/en/services-en/
- JP: https://1stclass.co.jp/services/
