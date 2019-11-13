# What is this repository (このレポジトリは何ですか)
This repository is for creating initial environment of developing WordPress's env with bedrock ( WordPress's best practice boilerplate https://roots.io/bedrock/ ) + docker, which will enable you to manage version of WordPress, themes and plugins through PHP's composer.

You can use this for initiation of all your WordPress's projects.


このレポジトリはWordpressをdockerで立ち上げられる形で、bedrock ( https://roots.io/bedrock/ )というWPの管理のベストプラクティスの仕組を活用して、本体・プラグイン・テーマのバージョン管理を、PHPのcomposerの仕組みの中で管理できるようにする仕組を提供しています。

composerを使う事で、プラグイン等のインストールについてはcomposer.jsonに記録する事でgit等のバージョン管理の中で管理し、再現&構築の仕方のチーム間の共有をする事が出来るようになります。

# Used software (使われているソフト)
- docker-compose
- MariaDB: latest
- PHP 7.2
- Wordpress
- Bedrock

## Make your WordPress's salt unique for security (WordPressのパスワードのSaltをユニークにする)

```
cd bedrock;
wp dotenv salts regenerate;
```

## Prepare necessary php modules (Wordpress本体含めPHPのPlugin/テーマの取得・更新)

```
cd bedrock;
composer.phar update;
```

Important thing is that you can update WordPress's set by typing composer update, so whether you use docker or not is not matter.

But docker will help you to construct local development environment at least.


尚、肝はbedrockディレクトリ内で、composer updateを打つ事で、composer.jsonの内容に対応してWordpress本体・テーマ・プラグインが更新される事なので、Dockerを立ち上げるはありません(Dockerも使えるというだけ)。

但し、Dockerは、貴方のPC上に開発環境を手軽に構築するのを手助けしてくれるでしょう。

## Add wordpress plugin (Wordpress Pluginの追加)
Edit bedrock/composer.json referencing to https://wpackagist.org/ and after editing, type composer.phar update.


bedrock/composer.jsonを https://wpackagist.org/ を参照しながら編集して、編集が終わったらcomposer.phar updateを叩いて下さい。


# Make WordPress environment available on your PC using Docker (Dcokerを活用してWordPress環境を立ち上げる)

If you want customize docker's setting, edit docker-compose.yml before "docker-compose up".

You can set password of systems and so on.

In the same directory with docker-compose.yml

```
docker-compose up -d;
```


パスワード等docker環境の設定をカスタマイズしたい場合、docker-compose upを打つ前にdocker-composer.ymlを編集して下さい

編集が終わったら、docker-compose.ymlと同じディレクトリで

```
docker-compose up -d;
```

と打って下さい

## Confirm web site (Local環境での作動の確認)
Access to  http://localhost/

Then you will see WordPress's initial setting screen.

You can develop using Docker's containers on your local PC and you can deploy the content just transfering source code with DB content using migration module like https://wordpress.org/plugins/all-in-one-wp-migration/


http://localhost/ にアクセスして作動を確認して下さい

WordPressの最初の設定画面が見れる筈です。

後はローカルPCでWordPressのコンテンツ整備をしたら

ソースコードとしてはこのレポジトリの中身をリモートのサーバーに配布し

DBの中身は

https://wordpress.org/plugins/all-in-one-wp-migration/

といったプラグインを使って、ローカルPCからリモートサーバーに移動させれば、リモートでも動かす事が出来ます。

## Stop running Docker containers for WordPress (WordPressのDockerコンテナを停止する)

```
docker-compose down;
```

# License (ライセンス)

MIT

# Author (作者)

Hajime Kurita

## Github
https://github.com/hikarine3

## JP Twitter
https://twitter.com/hikarine3

## EN Twitter
https://twitter.com/hajimekurita

## CN Tweet
https://www.weibo.com/u/7334273967

## JP Corporate page
https://1stclass.co.jp/

## EN Corporate page
https://1stclass.co.jp/en/

