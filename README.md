# Docker 勉強

- [GitHub リポジトリ](https://github.com/uchidayuma/udemy-docker)
- [Docker Hub](https://hub.docker.com/)

## 導入

- Docker が必要な理由
  - 環境構築時間の削減
  - アプリ実行環境の品質の向上
  - 自動化ソフトとの相性
- コンテナ
  - 隔離されパッケージされた箱
  - サーバー上に隔離されたアプリ空間を作れるもの
- Docker
  - コンテナ技術の一つ
- 仮想環境と Docker の違い
| 比較対象 | 仮想環境 | Docker |
| --- | --- | --- |
| 存在場所 | ディスク上 | メモリ上 |
| ゲストOS | あり | なし |
| データ管理 | 常に保存される | データは一時的 |
| コード化 | 不可 | Dockerfile でコード化可能（infrastructure as a code） |

- Docker 最大のメリット：インフラのコード化
  - ファイルを読むと構成がわかる
  - 誰でも同じ環境を作れる
  - 受け渡しが簡単
- Dockerfile →(build)→ image →(run)→ container
- タグは必ず指定する
  - イメージ名:タグ名
  - 例：ubuntu:18.04
  - latest は指定しない場合にデフォルトで指定されるがトラブルの元なので指定する
- docker compose：複数のコンテナを一括管理するツール

## 基本コマンド

## Docker コンテナのストレージ

- コンテナボリューム
  - Docker 運用で一番迷う部分
  - コンテナ内のボリュームは消える
  - データベースなどの永続データは使えない？
- 永続化の実現方法
  - ホストとディレクトリ共有で解決
  - コンテナが削除されてもホストに残る
- 具体的なデータ共有
  - docker run -v [ホストのディレクトリ]:[コンテナのディレクトリ] [イメージ名]
    ```
    docker run -v C:/Users/MK/PythonProjects/docker/test:/usr/share/nginx/html --name mynginx -p 8080:80 nginx:1.16
    ```
  - 確認（ブラウザ）
    - [http://localhost:8080](http://localhost:8080) で確認
  - 確認（別コンソールで実行）
    ```
    docker exec -it mynginx /bin/bash
    root@3874ab7092a7:/# cd /usr/share/nginx/html
    root@3874ab7092a7:/usr/share/nginx/html# ls
    --> index.html
    root@3874ab7092a7:/usr/share/nginx/html# cat index.html
    --> <h1>docker -v option</h1>
    ```
- 本番環境では
  - 複数のサーバーで動かす
  - サーバーごとにデータが異なるという問題（データの共有できない）
  - 対策
    - Amazon Aurora Database: 超高速のデータベースに集約
