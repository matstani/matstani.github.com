---
layout: post
title: "Clojureのuseをrequireで置き換える"
date: 2013-05-10 18:41
comments: true
categories: Clojure
---

Clojureで他の名前空間を参照する方法として、requireとuseがありますが、Clojure1.4以降では、require単独でuseと同様にプレフィックス無しでの参照ができるようになっています。

requireにまとめることで、コードが若干すっきりするかもしれません。  
[サンプルコード](https://gist.github.com/matstani/5553626)
{% gist 5553626 %}
