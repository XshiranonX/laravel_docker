# laravel_docker

## How to Use
### サーバ上で直接起動する場合
ディレクトリで
docker-compose up -d
起動後コンテナにログインし、/var/www/appで下記コマンドを実行
composer create-project laravel/laravel .

実行後アクセスしLaravelのTOP画面が表示されることを確認する
アクセス先:http://localhost:80

### vscodeのRemote-containersで起動する場合
Remote-containersで起動後、ターミナルで下記コマンドを実行
composer create-project laravel/laravel .

実行後アクセスしLaravelのTOP画面が表示されることを確認する
アクセス先:http://localhost:80


## FAQ
Permission deniedでTOP画面が表示されない場合
コンテナ内で下記コマンドを実行
chmod -R 777 vendor storage
echo "umask 000" | tee -a /etc/resolv.conf

実行後コンテナを再起動しアクセスしてみる
