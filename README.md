# laravel_docker

laravel環境構築用、及び簡単な動作確認用のテストアプリを入れている。<br>
アプリ内容は下記参照<br>
https://qiita.com/sano1202/items/6021856b70e4f8d3dc3d

# Environment
下記のバージョンで動くことは確認、よほど古くなければ大丈夫
```
Docker version 20.10.8, build 3967b7d
docker-compose version 1.29.0, build 07737305
```

## How to Use
### サーバ上で直接起動する場合
ディレクトリで
```
docker-compose up -d
```
コンテナ起動後、まずはDBでデータベースを作成する
```
# コンテナログイン
root@server:~/laravel_docker# docker-compose exec db bash
# mysqlにログイン
root@24093ffbb09f:/# mysql -u root -p
Enter password: <パスワードはdocker-compose.ymlに記載>
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 8
Server version: 8.0.28 MySQL Community Server - GPL

Copyright (c) 2000, 2022, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

# データベース作成
mysql> create database test_db;
```
次にappの設定を行う<br>
```
# コンテナログイン
root@db00:~/laravel_docker# docker-compose exec app bash
# envファイルを作成する
root@52d01418ee9a:/var/www/app# cp .env.example .env
# DBの設定部分のみ変更する
DB_CONNECTION=mysql
DB_HOST=<サーバIP>
DB_PORT=3306
DB_DATABASE=test_db
DB_USERNAME=root
DB_PASSWORD=password

# appの初期設定
root@52d01418ee9a:/var/www/app# composer update
# dbのマイグレート（テーブル作成）
root@52d01418ee9a:/var/www/app# php artisan migrate
# シーダー実行（テーブルに初期値が投入される）
root@52d01418ee9a:/var/www/app# php artisan db:seed
# keyの作成
root@52d01418ee9a:/var/www/app# php artisan key:generate
# ディレクトリのパーミッション変更
root@52d01418ee9a:/var/www/app# chmod -R 777 storage/
# apacheの再起動
root@52d01418ee9a:/var/www/app# /etc/init.d/apache2 reload
```
実行後下記にアクセスし画面が表示されることを確認する <br>
アクセス先:http://<サーバIP>/book

### vscodeのRemote-containersで起動する場合 
Remote-containersで起動後、やることは上記と一緒

## FAQ 
