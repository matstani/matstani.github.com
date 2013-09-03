---
layout: post
title: "Friendを利用したフォーム認証"
date: 2013-02-05 20:01
comments: true
categories: Clojure
---
[Compojure](https://github.com/weavejester/compojure)プロジェクトの作成手順は[こちら](/blog/2013/01/23/create-compojure-project/)

Compojureベースのウェブアプリケーションで、フォーム認証を行なってみます。

[Friend](https://github.com/cemerick/friend)という認証／認可用のライブラリを利用できます。

#### 依存ライブラリインストール
[project.clj](https://gist.github.com/matstani/5046194#file-project-clj)の:dependenciesにFriend、HTML生成ライブラリの[Hiccup](https://github.com/weavejester/hiccup)を追記します。
{% gist 5046194 project.clj %}

:dependenciesに追記後、以下のコマンドでライブラリをインストールします。
```
$ lein deps
```

#### Friendを使ってフォーム認証
仮アカウントとしてadmin、userを用意し、それぞれ閲覧できるページが異なる設定にしています。
認証に伴うリダイレクト処理はFriendが行ってくれます。  
[サンプルコード](https://gist.github.com/matstani/5046194#file-handler-clj)
{% gist 5046194 handler.clj %}
