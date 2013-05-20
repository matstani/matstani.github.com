---
layout: post
title: "Clojureアプリケーションを動的に書き換える"
date: 2013-05-20 12:45
comments: true
categories: Clojure
---

[nREPL](https://github.com/clojure/tools.nrepl)サーバをClojureアプリケーションに組み込んでおくと、動作中のClojureアプリケーションに接続して、デバッグを行ったり、動作を書き換えたりすることができます。

試しに、デスクトップGUIアプリケーションを動的に書き換えてみます。  
Clojureからは、JavaのSwingを直接利用できますが、せっかくなので、よりClojureっぽく書ける[Seesaw](https://github.com/daveray/seesaw)を使ってみます。

project.cljは以下のとおり。

{% gist 5610271 project.clj %}

アプリケーションのコードは以下のとおり。

{% gist 5610271 core.clj %}

6行目でnREPLサーバを起動しています。  

アプリケーションを起動
```
$ lein run
```

以下のようなウィンドウが表示されます。

![ウィンドウ](/images/blog/2013-05-20_123249.png)

「Show Message」ボタンを押すとメッセージが表示されます。

![ウィンドウ](/images/blog/2013-05-20_123337.png)

このアプリケーションにnREPLクライアントで接続し、動作を書き換えてみます。  
nREPLクライアントとして、[nrepl.el](https://github.com/kingtim/nrepl.el)を利用する場合、Emacs内で以下のコマンドを実行します。
```
M-x nrepl
Host: 127.0.0.1
Port: 7888
```
Portで指定するのは、アプリケーション中のstart-serverで指定したポート番号です。
以下のようにnREPLプロンプトが表示されます。
```
; nREPL 0.1.7
user> 
```
表示されるメッセージを変化させてみます。
```
user> (in-ns 'nrepl-test.core) ;namespaceセット
nrepl-test.core> (def msg "Changed Message.") ;msgを再定義
```
ボタンを押すと、メッセージが変化します。

![ウィンドウ](/images/blog/2013-05-20_162336.png )

ログ機能を付けてみます。
```
nrepl-test.core> (let [old show-msg-actn]
    (defn show-msg-actn
        [e]
        (println "LOG: show-msg-actn") (old e))) ;show-msg-actnを再定義
```

ボタンを押すと、コンソールにログが表示されるようになりました。
