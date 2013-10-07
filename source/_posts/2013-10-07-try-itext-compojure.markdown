---
layout: post
title: "[Clojure] iTextでPDFファイル作成(ウェブアプリケーション)"
date: 2013-10-07 20:52
comments: true
categories: Clojure
---
[Compojure](https://github.com/weavejester/compojure)ベースのClojureウェブアプリケーションで、[iText](http://itextpdf.com/)を利用したPDFファイル出力を行う手順をメモします。(ファイルに保存する例は[こちら](/blog/2013/10/07/clojure-itext-japanese/)。)  
例として、SQLiteデータベースから従業員データを読み取り、PDFファイルとして出力することを考えてみます。

## テスト用プロジェクト作成
```
$ lein new compojure try-itext-compojure
$ cd try-itext-compojure
```
`project.clj`の`:dependencies`に`[com.itextpdf/itextpdf "5.4.4"]` `[com.itextpdf/itext-asian "5.2.0"]` `[org.xerial/sqlite-jdbc "3.7.2"]` `[korma "0.3.0-RC5"]`を追記します。
```
$ lein deps
```

## データベース準備
```
$ mkdir db
$ sqlite3 db/try-itext.sqlite3 "CREATE TABLE employees (id INTEGER PRIMARY KEY AUTOINCREMENT, name TEXT)"
$ sqlite3 db/try-itext.sqlite3 "INSERT INTO employees (name) VALUES ('名無 一郎');"
$ sqlite3 db/try-itext.sqlite3 "INSERT INTO employees (name) VALUES ('名無 二郎');"
$ sqlite3 db/try-itext.sqlite3 "INSERT INTO employees (name) VALUES ('名無 三郎');"
```

## サンプルコード
[handler.clj](https://gist.github.com/matstani/6866740)
{% gist 6866740 %}

## 起動
```
$ lein ring server-headless
```
ブラウザでlocalhost:3000にアクセスするとPDFファイルが表示されます。
