---
layout: post
title: "[Clojure] forでネストしたマップを扱う"
date: 2015-12-09 12:31
comments: true
categories: 
---
Clojureで以下のようなネストしたデータ構造を扱いたい場合があります。
```
{:division1
 {:group1 [:staff1 :staff2]
  :group2 [:staff3 :staff4 :staff5]}
 :division2
 {:group3 [:staff6 :staff7]}}
```
ここからstaff要素を参照したい場合、手続き型言語ではforeach文を重ねるのが一般的だと思いますが、Clojureで`map`関数や`reduce`関数をネストすると非常に読みづらくなってしまいます。  
Clojureで構造や深さがあらかじめわかっているデータ構造を扱う場合は、`for`マクロが便利です。
以下のコードでネストしたキーと値の組み合わせが取り出せます。  
[nested.clj](https://gist.github.com/matstani/00c29777685713b09ac5)
<script src="https://gist.github.com/matstani/00c29777685713b09ac5.js"></script>
`reduce`と組み合わせると値の更新ができます。  
[nested.clj](https://gist.github.com/matstani/fa2a9fa3bbf457f9db9a)
<script src="https://gist.github.com/matstani/fa2a9fa3bbf457f9db9a.js"></script>

### その他の便利な関数 ###
#### 特定の位置の値を扱う　####
* [get-in](https://clojuredocs.org/clojure.core/get-in)
* [assoc-in](https://clojuredocs.org/clojure.core/assoc-in)
* [update-in](https://clojuredocs.org/clojure.core/update-in)

#### 深さがわからないデータ構造を扱う ####
* [clojure.walk.postwalk](https://clojuredocs.org/clojure.walk/postwalk)
* [clojure.walk.prewalk](https://clojuredocs.org/clojure.walk/prewalk)
