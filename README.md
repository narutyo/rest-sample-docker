## 開発環境のセットアップ

```	
$ git clone git@github.com:narutyo/rest-sample-docker.git
$ cd rest-sample-docker
$ git submodule update --init --recursive
$ git submodule foreach git checkout master
$ cd submodules/rest-sample-frontend && npm install && cd ../../
$ docker-compose up -d --build

# コンテナが立ち上がってから...
$ docker-compose exec rest-sample-api composer self-update
$ docker-compose exec rest-sample-api composer config -g repos.packagist composer https://packagist.jp
$ docker-compose exec rest-sample-api composer install
$ sudo find submodules/rest-sample-api/storage -type d -exec chmod 777 {} +
$ sudo find submodules/rest-sample-api/storage -type f -exec chmod 666 {} +
$ docker-compose exec rest-sample-api php artisan migrate --seed

# ２回目以降は --build 無しでOKです
$ cd docker
$ docker-compose up -d
```

## クライアント発行
```
# パスワード認証用クライアント発行（フロントエンド用）
$ docker-compose exec rest-sample-api php artisan passport:client --password

# PKCE認証用クライアント発行（client_secretなし）
$ php artisan passport:client --public
```
