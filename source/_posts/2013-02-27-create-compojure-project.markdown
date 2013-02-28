---
layout: post
title: "Clojureインストール、Compojureプロジェクト作成"
date: 2013-01-23 18:05
comments: true
categories: Clojure
---
[Clojure](http://clojure.org/)には、[Leiningen](http://leiningen.org/)という管理ツールがあり、Clojure本体のインストールや、各種ライブラリのインストール、プロジェクトの作成などがとても簡単にできるようになっています。

Clojureがインストールされていない状態から、ウェブアプリケーションフレームワーク[Compojure](https://github.com/weavejester/compojure)を利用したウェブアプリケーションの開発用プロジェクトを作成するまでの手順は以下のとおりです。

#### Javaがインストールされていることを確認
```
$ java -version
```

#### Leiningenインストール
##### leinスクリプトをダウンロード
```
$ curl -O https://raw.github.com/technomancy/leiningen/stable/bin/lein
```

##### ダウンロードしたスクリプトをPATHの通った場所に置き、実行権を付与
```
$ mv lein ~/bin
$ chmod 755 ~/bin/lein 
```

#### compojureプロジェクトの作成（初回は時間かかる）
```
$ lein new compojure helloworld 
```

####  開発用HTTPサーバ起動（初回は時間かかる）
```
$ cd helloworld
$ lein ring server-headless 
```

Port3000でHTTPサーバが起動されるので、ブラウザで確認します。