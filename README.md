# Messenger
環境用リポジトリ

* 管理画面: https://adm.msg.local.develop
* ユーザ画面: https://msg.local.develop
* API アクセス: https://api.messenger.local.develop

# 環境構築
## 前提条件
VM上に git と、Docker および Docker Compose がインストールされていること。
あるいは、Windows の場合は WSL2 の Linux をバックエンドとした Docker Desktop がインストールされていること。

## コンテナのビルド
環境に合わせて下記のファイルを参照するか、`docker-compose.yml` としてコピーを作成する。

* Linux / Mac 用: docker-compose.linux.yml
* Windows (WSL2)用: docker-compose.win.yml

~~~sh
docker-compose --file docker-compose.win.yml up -d --build
~~~

# CI
~~~
.github/workflows/docker-image.yml
~~~

Actions を使用。
`ubuntu-latest` にてテストを実施。

# hosts
~~~sh
127.0.0.1 msg.local.develop adm.msg.local.develop api.messenger.local.develop
~~~

※ `127.0.0.1` は各自の環境に合わせて変更すること。
