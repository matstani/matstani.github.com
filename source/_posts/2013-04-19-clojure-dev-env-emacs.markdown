---
layout: post
title: "Clojure開発環境インストール手順(Emacs)"
date: 2013-04-19 18:29
comments: true
categories: Clojure
---

推奨環境、手順はこまめに変化するようなので、2013年4月時点での手順のメモ。  
利用するのは、[Emacs](http://www.gnu.org/software/emacs/)+[nrepl.el](https://github.com/kingtim/nrepl.el)。  
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

### .emacsに以下追記
{% gist 5419420 %}
追記後、Emacs上で以下のコマンドを実行すると、直ちに有効になる。
```
M-x eval-buffer
```

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
M-x nrepl-jack-in
```
