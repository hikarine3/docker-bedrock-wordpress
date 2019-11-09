# What is this repository (このレポジトリは何ですか)
This is for creating initail environment of developing Wordpress env with bedrock + docker.
You can use this for initiation of your WP project.

このレポジトリはWordpressをdockerで立ち上げられる形で
bedrockという仕組を使って
本体・プラグイン・テーマのバージョン管理を
PHPのcomposerの仕組みの中で管理できるようにする仕組を提供しています。
composerを使う事で、プラグイン等のインストールについて、git等のバージョン管理の中で管理し、再現&構築の仕方のチーム間の共有をする事が出来るようになります。

# Used software (使われているソフト)
- docker-compose
- Mysql 5.7
- PHP 7.2
- Wordpress
- Bedrock (Fully coposer based WP dev env)

# How to make docker env up (Dcoker環境の立ち上げ)
After you make docker run on your machine, make docker-compose up

docker-compose up;

If you want customize docker's setting, edit docker-compose.yml before "docker up"

## Make your salt unique for security (パスワードのSaltをユニークにする)
cd bedrock;

wp dotenv salts regenerate;

## Prepare necessary php modules (Wordpress本体含めPHPのPlugin/テーマの取得・更新)
cd bedrock;

composer.phar update;

## Add wordpress plugin (Wordpress Pluginの追加)
Edit bedrock/composer.json referecing to https://wpackagist.org

## Confirm web site (Local環境での作動の確認)
Access to http://localhost/

# Reference: How this repository was created (参考情報: どのようにこのレポジトリが作られたか)
composer.phar create-project roots/bedrock bedrock;

cd bedrock;

wp package install aaemnnosttv/wp-cli-dotenv-command

wp dotenv salts regenerate

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

