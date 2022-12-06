# Docker コンテナのストレージ

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
    docker run -v $PWD/test:/usr/share/nginx/html --name mynginx -p 8080:80 nginx:1.16
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
