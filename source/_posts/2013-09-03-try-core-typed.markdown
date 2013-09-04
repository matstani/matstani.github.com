---
layout: post
title: "[Clojure] core.typedで静的型チェック"
date: 2013-09-03 13:25
comments: true
categories: Clojure
---
Clojureは動的型付けの言語ですが、[core.typed](https://github.com/clojure/core.typed)ライブラリを利用すると、静的型チェックを行うことができます。  
[バージョン0.2.0リリースで、Production Readyとアナウンスされました。](https://groups.google.com/forum/#!msg/clojure-core-typed/U_aA_Ce3qWg/gJWpdflBe4AJ)  
単純な例を試してみたのでメモします。  

## テスト用プロジェクト作成
```
$ lein new try-typed
$ cd try-typed
```
`project.clj`に`[org.clojure/core.typed "0.2.3"]`を追記します。
```
$ lein deps
```

## 静的型チェックの実行
`core.typed`を利用した静的型チェックは、プログラムのコンパイルとは別に、明示的に実行します。  
REPL内で`clojure.core.typed/check-ns`関数を実行することにより、指定したnamespaceをチェックします。
### 1. `REPL`起動
```
$ lein repl
```
### 2. 静的型チェック実行
先ほど作成したテスト用プロジェクトに対し、型チェックを行ってみます。
```
user=> (use 'clojure.core.typed)
user=> (check-ns 'try-typed.core)
```
上記を実行すると、以下のようにエラーになります。
```
Type Error (try-typed.core:3:1) Found untyped var definition: try-typed.core/foo
Hint: Add the annotation for try-typed.core/foo via check-ns or cf
in: (def foo (fn* ([x] (clojure.core/println x Hello, World!))))
```
`core.typed`による静的型チェックでは、全ての`var`が明示的に型付けされている必要があります。  
上記では、`try-typed.core/foo`関数に型付けがされていないため、エラーになっています。

## varに型付けを行う
`clojure.core.typed/ann`で、varに型付けを行うことができます。  
上記でエラーになった`try-typed.core/foo`に型付けしてみます。  
[core.clj](https://gist.github.com/matstani/6420121)
{% gist 6420121 core.clj %}
上記の例では、`try-typed.core/foo`に対し「`String`型の引数をとり`nil`を返す」という型情報を付与しています。  
この状態で型チェックを行うと、`:ok`となります。
```
user=> (check-ns 'try-typed.core)
Start collecting try-typed.core
Finished collecting try-typed.core
Collected 1 namespaces in 16.039067 msecs
Start checking try-typed.core
Checked try-typed.core in 65.01378 msecs
Checked 1 namespaces (approx. 8 lines) in 84.795137 msecs
:ok
```

## 型不一致エラー
`clojure.core.typed/ann`で指定した型付けと整合しない呼び出しがあった場合、エラーとして検出されます。  
例) [core2.clj](https://gist.github.com/matstani/6420121#file-core2-clj)
{% gist 6420121 core2.clj %}
```
user=> (check-ns 'try-typed.core)
Start collecting try-typed.core
Finished collecting try-typed.core
Collected 1 namespaces in 15.624342 msecs
Start checking try-typed.core
Checked try-typed.core in 33.318364 msecs
Type Error (try-typed.core:13:3) Type mismatch:

Expected:       String

Actual:         (Value 1.5)
in: (try-typed.core/foo 1.5)
```

## 型情報の参照
`clojure.core.typed/cf`で型情報を参照できます。
```
user=> (cf try-typed.core/foo)
(Fn [String -> nil])
```
標準関数についても、ほとんどがライブラリ内で型付けされています。
```
user=> (cf inc)
(Fn [AnyInteger -> AnyInteger] [Number -> Number])
```
自作関数と、標準関数の間で型の不整合があった場合には、エラーとして検出されます。  
例) [core3.clj](https://gist.github.com/matstani/6420121#file-core3-clj)
{% gist 6420121 core3.clj %}
```
user=> (check-ns 'try-typed.core)
Start collecting try-typed.core
Finished collecting try-typed.core
Collected 1 namespaces in 22.306404 msecs
Start checking try-typed.core
Checked try-typed.core in 36.899735 msecs
Type Error (try-typed.core:8:3) Static method clojure.lang.Numbers/inc could not be applied to arguments:


Domains:
        AnyInteger
        Number
                
Arguments:
        String
        
Ranges:
        AnyInteger
        Number
                
with expected type:
        AnyInteger
        
in: (clojure.lang.Numbers/inc s)
```
関数内で正しく型変換していると、チェックは`:ok`になります。  
例) [core4.clj](https://gist.github.com/matstani/6420121#file-core4-clj)
{% gist 6420121 core4.clj %}
```
user=> (check-ns 'try-typed.core)
Start collecting try-typed.core
Finished collecting try-typed.core
Collected 1 namespaces in 36.006706 msecs
Start checking try-typed.core
Checked try-typed.core in 38.304219 msecs
Checked 1 namespaces (approx. 9 lines) in 78.396396 msecs
:ok
```

## 型付けの例
### 引数が複数
```
(ann my-add [Number, Number -> Number])
(defn my-add [n1 n2]
  (+ n1 n2))
```
### 可変長引数
```
(ann my-add [Number * -> Number])
(defn my-add [& n]
  (apply + n))
```
### シーケンス
```
(ann my-add [(Seqable Number) -> Number])
(defn my-add [n]
  (apply + n))
```
### Set
```
(ann my-filter [(Set AnyInteger) -> (Seqable AnyInteger)])
(defn my-filter [n]
  (filter odd? n))
```
### Map
```
(ann get-count [(Map clojure.lang.Symbol Number) -> (U nil Number)])
(defn get-count [m]
  (m :count))
```

### 無名関数
高階関数に無名関数を渡す場合、型付けが必要になる場合があるようです。
`clojure.core.typed/fn>`による型付け
```
(ann inc-seq [Number, (Seqable Number) -> (Seqable Number)])
(defn inc-seq
  [i s]
    (map
       ; clojure.core.typed/fn> は無名関数のパラメータに型情報を付加できる
       (fn> [e :- Number] (+ i e))
       s))
```
`clojure.core.typed/ann-form`による型付け
```
(ann inc-seq [Number, (Seqable Number) -> (Seqable Number)])
(defn inc-seq
  [i s]
    (map
       ; clojure.core.typed/ann-form> は任意のフォームに型情報を付加できる
       (ann-form #(+ i %) [Number -> Number])
       s))
```

## その他
Clojureには標準で型ヒント([type hinting](http://clojure.org/java_interop#Java%20Interop-Type%20Hints))と呼ばれる機能がありますが、Javaを呼び出す際のリフレクションを防ぐためのもので、`core.typed`のような、静的型チェックの機能ではありません。
