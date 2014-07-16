---
layout: post
title: "Clojure開発環境インストール手順(Emacs)"
date: 2013-04-19 18:29
comments: true
categories: Clojure
---

推奨環境、手順はこまめに変化するようなので、2013年4月時点での手順のメモ。  
※2014/07/16 nrepl.el→ciderに変更、リンク先修正。  
利用するのは、[Emacs](http://www.gnu.org/software/emacs/)+[cider](https://github.com/clojure-emacs/cider)。  
ちなみに、[Clojure Programming](http://shop.oreilly.com/product/0636920013754.do)で紹介されているSLIME, swankの利用はdeprecatedとのこと。

### Javaのインストール
* [オラクルのサイト](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
* OpenJDKをインストールする場合  

Ubuntuの場合
```
$ sudo apt-get install openjdk-7-jdk
```
CentOSの場合
```
$ sudo yum install java-1.7.0-openjdk
```

### leiningenインストール
```
$ curl -O https://raw.github.com/technomancy/leiningen/stable/bin/lein  
$ mv lein ~/bin  
$ chmod 755 ~/bin/lein  
```
### Emacsのインストール
Emacs24以降のバージョンを利用すると、パッケージ管理システムが標準で同梱されているので、以降の手順が簡単。

* [Ubuntuにインストール](http://gihyo.jp/admin/serial/01/ubuntu-recipe/0235)
* [CentOSにインストール](http://dqn.sakusakutto.jp/2012/06/centos62emacs241install.html)

### [.emacs](https://gist.github.com/matstani/5419420)に以下追記
{% gist 5419420 %}
追記後、Emacs上で以下のコマンドを実行すると、直ちに有効になる。
```
M-x eval-buffer
```

ここでインストールされるパッケージは以下のとおり。

* [clojure-model](https://github.com/clojure-emacs/clojure-mode)  
Clojure編集の基本機能を追加するモード。インデントやシンタックスハイライトなど。
* [ParEdit](http://emacswiki.org/emacs/ParEdit)  
括弧のペアを管理してくれるモード。開き括弧の入力時に自動的に閉じ括弧を入れてくれたり、括弧(S式)単位での編集ができたり。  
動作が直感的でないところもあるので、[チュートリアル](http://www.daregada.sakuraweb.com/paredit_tutorial_ja.html)で勉強。
* [cider](https://github.com/clojure-emacs/cider)  
Emacsから、[nREPL](https://github.com/clojure/tools.nrepl)(ネットワーク経由で利用できるREPL)にアクセスするための機能。  
nREPLサーバをアプリケーションに組み込んでおくと、動作中のアプリケーションの情報を取得したり、動的にパッチを当てたり、色々できる。
* [RainbowDelimiters](http://www.emacswiki.org/emacs/RainbowDelimiters)  
Lispにありがちな多重括弧を、キレイに色付けしてくれるモード。

### Clojureプロジェクト作成
```
$ lein new demo
```

### Emacs上でnREPL起動
```
$ cd demo
$ emacs src/demo/core.clj
```
Emacs上で
```
M-x cider-jack-in
```
