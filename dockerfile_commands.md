# Dockerfile

- Docker イメージをコード化したもの
- コンテナはエフェメラル（状態を持たない）であるべき
- 不要なパッケージのインストールを避ける
- コンテナごとに一つのプロセスだけ実行する
- レイヤの数を最小にする

## 主要コマンド

### ■ RUN と CMD

- どちらもコマンドを実行するが、実行タイミングが異なる
- RUN：Dockerfile → Docker イメージ
- CMD：Docker イメージ → Docker コンテナ
```
docker build -t dockerfile-run-nginx ./dockerfiles/run
docker run -d -p 8081:80 --name dockerfile-run-nginx dockerfile-run-nginx
```
- [localhost:8081](http://localhost:8081/) にアクセスして確認

### ■ COPY と ADD

- どちらもファイルをイメージに追加する
- Add：ネット経由でも追加できる
- COPY：基本はローカルからの追加なので、推奨
- 設定ファイルを予め入れておき、設定ファイルが反映された状態でイメージを作成したい場合にも使用
```
docker build -t dockerfile-copy-nginx ./dockerfiles/copy
docker run -d -p 8082:80 --name dockerfile-copy-nginx dockerfile-copy-nginx
```
- [localhost:8082](http://localhost:8082/) にアクセスして確認

### ■ ENV

- 環境変数を設定する
- 直接書き込みなので固定値になってしまう
```
docker build -t dockerfile-env-nginx ./dockerfiles/env
docker run -d -p 8083:80 --name dockerfile-env-nginx dockerfile-env-nginx
docker inspect dockerfile-env-nginx | grep ENV
--> "TEST_ENV=hage", "APP_ENV=production"
```

## MariaDB を作成する

- Dockerfile の実践演習
  - 設定ファイルをイメージに入れる
  - デフォルトデータベースを作成する
  - 固定の環境変数を定義
- DB を立ち上げてテーブルを確認する
```
$ docker build -t mymariadb ./dockerfiles/mariadb
$ docker run -d --name mymariadb mymariadb
$ docker exec -it mymariadb /bin/bash
root@ab6a1b4bbfef:/# mysql -u root -p
Enter password:root
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 8
Server version: 10.4.27-MariaDB-1:10.4.27+maria~ubu2004 mariadb.org binary distribution

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> show databases;
+--------------------+
| Database           |
+--------------------+
| docker             |
| information_schema |
| mysql              |
| performance_schema |
+--------------------+
4 rows in set (0.000 sec)

MariaDB [(none)]> use docker;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
MariaDB [docker]> show tables;
+------------------+
| Tables_in_docker |
+------------------+
| persons          |
+------------------+
1 row in set (0.000 sec)
