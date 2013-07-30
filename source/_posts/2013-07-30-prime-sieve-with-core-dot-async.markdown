---
layout: post
title: "clojure/core.asyncでGo"
date: 2013-07-30 21:01
comments: true
categories: Clojure
---

[clojure/core.async](https://github.com/clojure/core.async)ライブラリを利用すると、[Go言語](http://golang.org)のGoroutineと同様に、チャネルを介した平行プログラミングを行うことができます。  

試しに、[Go言語チュートリアルに掲載されている素数の篩(ふるい)](http://golang.jp/go_tutorial#index12)をClojureで実装してみました。  

project.clj
{% gist 6112340 project.clj %}

core.clj
{% gist 6112340 core.clj %}

Go言語版をほぼそのままの形で実装できています。  
他言語の構文をライブラリとして提供できるのはLispならではです。

ちなみに、[ClojureScript](https://github.com/clojure/clojurescript)(Clojure-JavaScriptコンパイラ)でも利用可能とのことです。
