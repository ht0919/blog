# Elixir+PhoenixによるWebアプリの作例(macOS編)

## 動作環境(バージョン)
- macOS: 10.13
- PostgreSQL: 9.6.5
- Node.js: 8.7.0
- Elixir: 1.5.2
- Phoenix: 1.3.0

## 準備(環境構築)

1. Homebrewのインストール
```
$ xcode-select --install
$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

2. PostgreSQLのインストールとアカウント登録
```
$ brew install postgresql
$ pg_ctl -D /usr/local/var/postgres -l /usr/local/var/log/postgres start
$ createuser -P -d postgres
  ※パスワードもpostgresで登録
```

3. Node.jsのインストール
```
$ brew install node
```

4. Elixirのインストール
```
$ brew install elixir
$ mix local.hex
```

5. Phoenixのインストール
```
$ mix archive.install https://github.com/phoenixframework/archives/raw/master/phoenix_new.ez
```

## Webアプリの作成(1)

1. プロジェクトの作成とディレクトリの移動
```
$ mix phoenix.new blog
$ cd blog
```

2. データベースの作成
```
$ mix ecto.create
```

3. サーバーの起動と動作確認
```
$ mix phoenix.server
```
  - ブラウザで「[http://localhost:4000/](http://localhost:4000/)」を表示
  - Ctrl+Cを2回押してサーバーを停止

  ![Screenshots-1](https://raw.github.com/ht0919/blog/blob/master/images/img01.png)


## Webアプリの作成(2)

1. CRUDコードの作成(Scaffold)
```
$ mix phoenix.gen.html Post posts title:string body:text
```

2. HTTPアクセスポイントの追加
  - テキストエディタで「web/router.ex」を開く
  - 19行目の「get "/", PageController, :index」の下に「resources "/posts", PostController」を追加して保存する
```
19:    get "/", PageController, :index
20:    resources "/posts", PostController
21:  end
```

3. モデルのマイグレーション
```
$ mix ecto.migrate
```

4. サーバーの起動と動作確認
```
$ mix phoenix.server
```
  - ブラウザで「[http://localhost:4000/posts](http://localhost:4000/posts)」を表示
  - 新規登録(New post)、詳細表示(Show)、変更(Edit)、削除(Delete)を一通り試す
  - Ctrl+Cを2回押してサーバーを停止

  ![Screenshots-2](https://raw.github.com/ht0919/blog/blob/master/images/img02.png)


## 補足

1. PostgreSQLの開始スクリプトの作成と実行
```
$ echo "pg_ctl -D /usr/local/var/postgres -l /usr/local/var/log/postgres start" > pg_start.sh
$ . pg_start.sh
```

2. PostgreSQLの停止スクリプトの作成と実行
```
$ echo "pg_ctl -D /usr/local/var/postgres stop" > pg_stop.sh
$ . pg_stop.sh
```
