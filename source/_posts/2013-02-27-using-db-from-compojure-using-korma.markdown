---
layout: post
title: "KormaでDBアクセス"
date: 2013-02-27 23:34
comments: true
categories: Clojure
---
[Compojure](https://github.com/weavejester/compojure)プロジェクトの作成手順は[こちら](http://matstani.github.com/blog/2013/02/27/create-compojure-project/)

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
``` clojure
(defproject helloworld "0.1.0-SNAPSHOT"
  :description "FIXME: write description"
  :url "http://example.com/FIXME"
  :dependencies [[org.clojure/clojure "1.4.0"]
                 [compojure "1.1.5"]
                 [org.xerial/sqlite-jdbc "3.7.2"]
                 [korma "0.3.0-RC2"]
                 [hiccup "1.0.2"]]
  :plugins [[lein-ring "0.8.2"]]
  :ring {:handler helloworld.handler/app}
  :profiles
  {:dev {:dependencies [[ring-mock "0.1.3"]]}})
```

:dependenciesに追記後、以下のコマンドでライブラリをインストールします。
```
$ lein deps
```

#### Kormaを使ってデータベースから読み出し
{% gist 5048794 %}