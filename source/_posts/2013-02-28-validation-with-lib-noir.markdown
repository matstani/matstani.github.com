---
layout: post
title: "lib-noirを利用したフォームバリデーション"
date: 2013-02-28 13:02
comments: true
categories: Clojure
---
[Compojure](https://github.com/weavejester/compojure)プロジェクトの作成手順は[こちら](/blog/2013/01/23/create-compojure-project/)

Compojureベースのウェブアプリケーションで、フォームバリデーションを行います。

ウェブアプリケーション用ライブラリ[lib-noir](https://github.com/noir-clojure/lib-noir)を利用するコードを紹介します。

#### 依存ライブラリのインストール
lib-noirとHTML生成ライブラリの[Hiccup](https://github.com/weavejester/hiccup)を追記します。  
[project.clj](https://gist.github.com/matstani/5055544#file-project-clj)
{% gist 5055544 project.clj %}

:dependenciesに追記後、以下のコマンドでライブラリをインストールします。
```
$ lein deps
```

#### lib-noir/validationを使ってフォームバリデーション
フォームからPOSTされた日付データ、時刻データを、正規表現でチェックするコードです。バリデーションが失敗した場合は、エラーメッセージを表示しています。  
[handler.clj](https://gist.github.com/matstani/5055544#file-handler-clj)
{% gist 5055544 handler.clj  %}
