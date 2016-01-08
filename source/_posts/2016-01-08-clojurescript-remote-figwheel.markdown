---
layout: post
title: "[ClojureScript] リモートのfigwheelに接続する"
date: 2016-01-08 14:09
comments: true
categories: Clojure,ClojureScript
---
ClojureScriptの開発ツール[Figwheel](https://github.com/bhauman/lein-figwheel)を利用すると、ソースコードの変更時に自動的にブラウザにロードされるほか、付属のREPLでは、コンソールに入力したClojureScriptコードをブラウザ上で実行することができます。  
この時に利用されるWebSocket接続ですが、デフォルトでは接続先がlocalhostになっています。  
figwheelプロセスをリモートサーバで実行している場合、デフォルトでは以下のようなエラーがブラウザのJavaScriptコンソールに表示されます。

```
Figwheel: trying to open cljs reload socket
ws://localhost:3449/figwheel-ws/flappy-bird-demo のサーバへの接続を確立できませんでした。
```
接続先のホストは、`project.clj`でキー`{:cljsbuild {:builds [{:figwheel {:websocket-host 接続先}]}}`で指定できます。  
リモートサーバのホスト名やIPアドレスを直接記述することもできますが、`:js-client-host`を指定すると、`location.host`の値を設定してくれるので便利です。  
設定例)  
[project.clj](https://gist.github.com/matstani/4db238e1db767ca8de9b)  
<script src="https://gist.github.com/matstani/4db238e1db767ca8de9b.js"></script>
[README](https://github.com/bhauman/lein-figwheel)にあるのですが、見逃していました。
