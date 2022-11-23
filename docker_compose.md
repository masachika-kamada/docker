# docker-compose

- 複数のコンテナを管理するためのツール
- --link オプションで wordpress を立ち上げてみる
  ```
  docker run --name wordpressdb -e MYSQL_ROOT_PASSWORD=password -e MYSQL_DATABASE=wordpress -d mysql:5.6
  docker run -p 8080:80 -e WORDPRESS_DB_PASSWORD=password -d --name wordpress --link wordpressdb:mysql wordpress:4.7-php5.6
  ```
  - 上記のように複数のコンテナを立ち上げるのは面倒
- docker-compose
  - 複数コンテナをまとめて管理
  - yaml ファイルの設計図
  - コンテナの立ち上げを自動化
- yaml ファイル
  - スペースで親子関係を表現するファイル形式
  - 設定ファイルなどによく使われる
  - kubernetes などでも使われている
  - できるだけ相対パスで書く

## docker-compose.yml の書き方

- image と build
  - image は docker hub から取得する
  - build は Dockerfile のパスを指定する
- container_name
  - コンテナに任意の名前をつける
  - docker run --name と同じ
- volume
  - コンテナとホストにディレクトリ共有
  - \[ホストのパス]:[コンテナのパス]
  - docker run -v と同じ
- ports
  - コンテナのポート開放とポートフォワーディング
  - \[外部公開ポート]:\[コンテナ内部ポート]
  - docker run -p と同じ
- コンテナ間通信の方法
  - サービス名がIPアドレスに変換される

## docker-compose で wordpress を立ち上げる

```
$ cd docker-compose_wordpress
$ docker-compose up -d
$ docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED         STATUS         PORTS                  NAMES
db196af2bc6d   mariadb:10.4   "docker-entrypoint.s…"   3 minutes ago   Up 3 minutes   3306/tcp               wordpress-db
b680642c93d7   wordpress:5    "docker-entrypoint.s…"   3 minutes ago   Up 3 minutes   0.0.0.0:8000->80/tcp   wordpress-wp
```
- [localhost:8000](http://localhost:8000) にアクセスして wordpress が立ち上がっていることを確認

## docker-compose コマンド

- down
  - docker-compose 管理されているコンテナを停止、削除
  - カレントディレクトrにの docker-compose.yml のみ適用
  - もしカレントディレクトリに docker-compose.yml がない場合は親ディレクトリを探しに行く
- restart
  - docker-compose up で立ち上げたコンテナを再起動
- ps
  - docker-compose.yml で管理されているコンテナの一覧を表示
- run
  - docker-compose.yml で管理されているサービス一つを指定してコマンドを実行

## docker-compose を使った Laravel 環境の構築

```
$ cd docker-compose_laravel
$ mkdir data
$ docker-compose run php composer create-project --prefer-dist laravel/laravel .
$ docker-compose up -d
$ docker ps
```
- [localhost:8080](http://localhost:8080) にアクセスして Laravel が立ち上がっていることを確認
- 次に DB との接続を確認
```
$ docker exec -it laravel-php bash
root@f171cfd96aa2:/var/www/html# php artisan migrate
Migration table created successfully.
Migrating: 2014_10_12_000000_create_users_table
Migrated:  2014_10_12_000000_create_users_table (71.60ms)
Migrating: 2014_10_12_100000_create_password_resets_table
Migrated:  2014_10_12_100000_create_password_resets_table (40.83ms)
Migrating: 2019_08_19_000000_create_failed_jobs_table
Migrated:  2019_08_19_000000_create_failed_jobs_table (42.91ms)
Migrating: 2019_12_14_000001_create_personal_access_tokens_table
Migrated:  2019_12_14_000001_create_personal_access_tokens_table (56.59ms)
```
