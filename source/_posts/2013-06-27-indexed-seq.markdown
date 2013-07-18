---
layout: post
title: "[Clojure] インデックス付きシーケンス"
date: 2013-06-27 16:57
comments: true
categories: Clojure
---
Clojure1.2以前の、[clojure.contrib.seq/indexed](http://clojuredocs.org/clojure_contrib/clojure.contrib.seq/indexed)の代替として、インデックス付きシーケンスがほしいときは、以下のコードが使えます。
```
> (map-indexed vector [:a :b :c :d :e])
;([0 :a] [1 :b] [2 :c] [3 :d] [4 :e])
```

例)doseqでループ処理。
```
> (doseq [[idx item] (map-indexed vector [:a :b :c :d :e])]
        (println (str idx ":" item)))
;0:a
;1:b
;2:c
;3:d
```
