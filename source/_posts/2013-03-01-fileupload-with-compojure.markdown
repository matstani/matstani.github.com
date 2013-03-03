---
layout: post
title: "Compojureでファイルアップロード"
date: 2013-01-31 21:50
comments: true
categories: 
---
[Compojure](https://github.com/weavejester/compojure)プロジェクトの作成手順は[こちら](/blog/2013/01/23/create-compojure-project/)

Compojureベースのウェブアプリケーションで、ファイルアップロードを行います。

#### 依存ライブラリのインストール
HTML生成ライブラリとして[Hiccup](https://github.com/weavejester/hiccup)を利用するので、以下のように追記します。
{% gist 5064493 project.clj %}

:dependenciesに追記後、以下のコマンドでライブラリをインストールします。
```
$ lein deps
```

#### ファイルアップロード
アップロードされたファイルを、プロジェクトのルートディレクトリに保存するコードです。
{% gist 5064493 handler.clj %}