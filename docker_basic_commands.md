# Docker 基本コマンド

## docker run

- docker run [オプション] [イメージ名] [コマンド]
  - --name: コンテナ名を指定
  - --rm: コンテナを終了すると同時に削除
- 例
  ```
  docker run --name hoge hello-world
  docker run  --rm hello-world
  ```

## docker start / stop / restart

- docker start [コンテナ名]: コンテナを起動
- docker stop [コンテナ名]: コンテナを停止
- docker restart [コンテナ名]: コンテナを再起動（停止してから起動）
- 例
  ```
  docker run -it --name mycentos centos:8 /bin/bash
  [root@1c96edcad68c /]# exit
  docker start mycentos
  docker stop mycentos
  docker restart mycentos
  ```

## docker exec

- 起動中のコンテナ内でコマンドを実行
- 起動中のコンテナに入らずにコマンドを実行
  ```
  docker exec -it mycentos /bin/bash
  cat /etc/redhat-release
  --> CentOS Linux release 8.4.2105
  ```
- -itオプションを使うとコンテナに入れる
- コンテナに入る：コンテナ内のコマンドラインにアクセスすること
  ```
  docker exec mycentos cat /etc/redhat-release
  --> CentOS Linux release 8.4.2105
  ```

## docker rm

- 停止中のコンテナを削除する
- -f オプションをつけると起動中のコンテナも削除できる
- データも削除されるので注意

## docker images

- ローカルにある image を全て表示する
- Docker イメージは容量が大きいので、不要なイメージは定期的に削除する

## docker rmi

- image を削除する
- 起動中のコンテナのイメージは削除できない
- Docker イメージは依存関係があるので、ベースイメージは削除できない

## docker build

- Dockerfile から image を作成する

## docker cp

- コンテナとホストマシンでファイルのやりとりを行うコピーコマンド
- ログファイルや設定ファイルの取り出し or 入力で使う
- ホスト→コンテナへコピー
  - docker cp [ホストのファイルパス] [コンテナ名]:[コンテナのファイルパス]
  ```
  docker cp ./basic/hoge mycentos:/opt
  docker exec mycentos cat /opt/hoge
  --> fuga
  ```
- コンテナ→ホストへコピー
  - docker cp [コンテナ名]:[コンテナのファイルパス] [ホストのファイルパス]
  ```
  docker exec -it mycentos /bin/bash
  [root@1c96edcad68c /]# vi /opt/piyo
  [root@1c96edcad68c /]# cat /opt/piyo
  --> piyopiyo
  [root@1c96edcad68c /]#exit
  docker cp mycentos:/opt/piyo ./basic
  ```

## docker logs

- Docker コンテナのログを確認する
- -f オプションをつけるとリアルタイムでログを確認できる
- 原因不明のコンテナ停止やアクセスログの確認に使う

## docker inspect

- コンテナやイメージの詳細情報を確認する
- 普段は見ない詳細情報を見れるためトラブル時に使う
- 例
  ```
  docker inspect mycentos
  ```

## docker pull

- Docker イメージのダウンロード
- docker pull [イメージ名]:[タグ名] [レジストリURL]
- pull の後ろにプライベートレジストリを付けると、Docker Hub 以外のレジストリからイメージを取得できる

## docker commit

- コンテナをイメージ化
- docker commit [コンテナ名] [DockerHubID]/[イメージ名]:[タグ名]

## docker push

- Docker イメージを Docker Hub にアップロード

## docker history

- Docker イメージの履歴を確認する
