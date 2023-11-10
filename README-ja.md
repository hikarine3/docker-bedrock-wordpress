## 特徴
このレポジトリはWordPressで将来の運用を見越した開発を行うにあたっての、ベストな構成を提供するWordPress開発テンプレートとなっています。
- WordPress本体、プラグイン、テーマ全てがPHPのcomposerでインストール・更新できます。composer.jsonの定義に従ってインストールされるので、構成をGit等でバージョン管理し、環境再現が簡単になります。
- Dockerがサポートされており、必要なら簡単にDockerでWordPressを立ち上げ、開発する事が出来ます。
- 複数のWordPressの環境を、同一ソースコード・同一サーバー上に、簡単に立ち上げる事が出来ます。
1) ドメイン名毎に変えるパターン
2) ディレクトリ毎に変えるパターン
両方をサポートします。
- どんな環境でもインストール・有効化するに値するPluginが最初からcomposer.jsonに定義されています

このWordPressのインストール方法は「[WordPressの利用法/設定/有用プラグイン/テーマ/開発法まとめ](https://vpshikaku.com/wordpress/)」で紹介されてる方法の内、上級者向けの方法になります。  
上級者向け(=エンジニア向け)ですが、使いこなせれば、相当便利な手法になります。
このページは「英語 => 日本語」と、同じ内容が別言語で説明されてる構成になっています。  
ご質問がありましたら、このレポジトリの管理者 [@hikarine](https://twitter.com/hikarine3) にTwitterでフォローしてDMでお問合せ下さい。

# 使われているソフト
- Docker https://www.docker.com/products/docker-desktop
- docker-compose https://docs.docker.com/compose/install/
- MariaDB docker: https://hub.docker.com/_/mariadb
- Apache 2.4 & PHP 8.2 docker: https://hub.docker.com/layers/library/php/8.2-apache/images/sha256-89ad17cca246e8a6ce742b5b89ce65b34ce6223204a282e45f72b4f758ff6401?context=explore
- WordPress: https://wordpress.org/download/
- Bedrock: https://roots.io/bedrock/ 2023/09/28 ver https://github.com/roots/bedrock/commit/a90180a1a0c3b57f7ed561ddb65aca46de839c86

# インストール手順
```
# Download github repository ( githubのレポジトリーをダウンロード ) #
git clone git@github.com:hikarine3/docker-bedrock-wordpress.git;
cd docker-bedrock-wordpress;
cd bedrock;

./composer.phar update;

# Saltをユニークに更新してセキュリティ向上
# 初回のみ
wp dotenv salts regenerate;
```

# 開発環境の立ち上げ
まず貴方はDockerのインストールをして、それを走らせておく必要があります。

その上で、docker-bedrock-wordpressディレクトリの中=docker-composer.ymlが置いてあるディレクトリで、

```
docker-compose up -d;
```

と打ってください。

それからコピペする形で
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
をタイプ。


エラーを見たければ
```
docker logs -f `docker ps|grep php|awk '{print $1}'`
```

http://localhost/

でWordPressの設定画面を見る事が出来ます。


# 各コンテナへログイン
MariaDBには、docker-compose.ymlがデフォルトの設定のままなら
```
docker exec -i -t `docker ps|grep mariadb|awk '{print $1}'` /bin/bash
```
でろ
でdocker経由だとログインできます。

Docker環境を立ち上げる前に、ユーザーとパスワードを変更してたら、それに合わせて変更して下さい。


Apache+PHP環境へのログインには
```
docker exec -i -t `docker ps|grep php|awk '{print $1}'` /bin/bash
```
と打って下さい。

# dockerの操作
docker環境を止めたければ

```
docker-compose down;
```

と叩いて下さい。


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

管理画面から追加せず、あくまでcomposer.jsonの編集とcomposer updateだけで管理するのが、ソースコードでWordPressの構成管理を行い切るコツです。

これにより、WordPressのプラグインのインストールで真っ白になってしまったとしても、簡単にcomposer.jsonを編集してcomposer updateするだけで、問題のプラグインを元に戻す事も出来ますし、問題の解決もより早く行えます。


# 更なるWordPress環境の追加
## /u$数字/　　を使う場合

bedrock/.env
からコピーする形で
.env.u数字
というファイルを作る

なお、このdockerには例として
.env.u1
が用意されている

MariaDBにrootユーザーでログインして、追加データベースを作成し、権限を付与。

例えば
DB_NAME=u1
DB_USER=${WORDPRESS_DB_USER}
DB_PASSWORD=${WORDPRESS_DB_PASSWORD}
のためのDBを追加するには、docker-composer.ymlがデフォルトのままなら

```
docker exec -i -t `docker ps|grep mariadb|awk '{print $1}'` mariadb --user=root --password=ExampleRootPass 
```
でDBにログインしてから

```
CREATE DATABASE u1 character set utf8mb4;
```

また、ユーザーに作ったデータベースへのアクセス権を付与します。
```
GRANT ALL PRIVILEGES ON u1.* TO'wpdbuser'@'%';
```



それが終わったら
```
docker exec -i -t `docker ps|grep php|awk '{print $1}'` /bin/bash -c  'mkdir -p /var/www/web;cd /var/www/web;ln -s ../web u1';
```


```
docker-compose down
```

で一旦docker環境を止めて

```
docker-compose up -d
```

で再起動して下さい。

上記の例でしたら

http://localhost/u1/

が新たに使えるようになります。


## ドメイン　を使う場合
上記uのパターンに当てはまらない場合に使えます

bedrock/.env
からコピーする形で
.env.ドメイン名
というファイルを作る


なお、このdockerには例として
.env.example.com
が用意されている


MariaDBにrootユーザーでログインして、追加データベースを作成し、権限を付与。

例えば
DB_NAME=example
DB_USER=${WORDPRESS_DB_USER}
DB_PASSWORD=${WORDPRESS_DB_PASSWORD}
WP_HOME=http://example.com
のためのDBを追加するには、docker-composer.ymlがデフォルトのままなら

```
docker exec -i -t `docker ps|grep mariadb|awk '{print $1}'` mariadb --user=root --password=ExampleRootPass 
```
でDBにログインしてから

```
CREATE DATABASE example character set utf8mb4;
```

また、ユーザーに作ったデータベースへのアクセス権を付与します。
```
GRANT ALL PRIVILEGES ON example.* TO'wpdbuser'@'%';
```

http://example.com/

にアクセスすれば、WordPressの画面を見れます。


# 参考: OriginalのBedrockに対する改変
ここは貴方が打つ必要はありませんが、このような変更が元々のbedrockに加えられているということです。

```
cd bedrock;
rm -rf html;
ln -s web html;

ln -s ../composer.phar composer.phar
```

berock/config/application.php
の書き換え。
オリジナルは
berock/config/application.php.org
として置いてあります。

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
