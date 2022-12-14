# 導入

- Docker が必要な理由
  - 環境構築時間の削減
  - アプリ実行環境の品質の向上
  - 自動化ソフトとの相性
- コンテナ
  - 隔離されパッケージされた箱
  - サーバー上に隔離されたアプリ空間を作れるもの
- Docker
  - コンテナ技術の一つ

＜仮想環境と Docker の違い＞

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
