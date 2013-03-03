---
layout: post
title: "KormaでDBアクセス"
date: 2013-01-23 19:41
comments: true
categories: Clojure
---
[Compojure](https://github.com/weavejester/compojure)プロジェクトの作成手順は[こちら](/blog/2013/01/23/create-compojure-project/)

Compojureベースのウェブアプリケーションで、データベースから読みだした値を表示してみます。

[ClojureのJDBCラッパー](http://clojure.github.com/java.jdbc/)も十分に高機能なのですが、より柔軟にSQLを構築できる[Korma](https://github.com/korma/Korma)というライブラリを使う手順を紹介します。なお、以下の手順では[SQLite](http://www.sqlite.org/)を使っています。

#### データベース準備
```
$ mkdir db
$ sqlite3 db/helloworld.sqlite3 "CREATE TABLE items (id INTEGER PRIMARY KEY AUTOINCREMENT, title TEXT);"
$ sqlite3 db/helloworld.sqlite3 "INSERT INTO items (title) VALUES ('最初のアイテム');"
$ sqlite3 db/helloworld.sqlite3 "INSERT INTO items (title) VALUES ('2つ目のアイテム');"
```

#### 依存ライブラリインストール
project.cljの:dependenciesにJDBCドライバーとKorma、HTML生成ライブラリの[Hiccup](https://github.com/weavejester/hiccup)を追記します。
{% gist 5048794 project.clj %}

:dependenciesに追記後、以下のコマンドでライブラリをインストールします。
```
$ lein deps
```

#### Kormaを使ってデータベースから読み出し
{% gist 5048794 handler.clj %}