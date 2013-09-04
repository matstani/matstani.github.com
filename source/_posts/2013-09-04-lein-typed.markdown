---
layout: post
title: "[Clojure] lein-typed"
date: 2013-09-04 17:57
comments: true
categories: Clojure
---
[lein-typed](https://github.com/frenchy64/lein-typed)を利用すると、[core.typed](https://github.com/clojure/core.typed)ライブラリを利用したClojureプログラムの静的型チェックを[Leiningen](https://github.com/technomancy/leiningen)タスクとして実行できます。

## インストール
### 全てのプロジェクトで利用する場合
`~/.lein/profiles.clj`の`:user` `:plugins`に`[lein-typed "0.3.0"]`を追記します。(ファイルが存在しない場合は新規作成)  
例)
```
{:user {:plugins [[lein-typed "0.3.0"]]}}
```
### 特定のプロジェクトで利用する場合
`project.clj`の`:plugins`に`[lein-typed "0.3.0"]`を追記します。

## 型チェックの実行
`project.clj`にチェック対象となるnamespaceを記述します。  
例)
```
(defproject try-typed "0.1.0-SNAPSHOT"
  :description "FIXME: write description"
  :url "http://example.com/FIXME"
  :license {:name "Eclipse Public License"
  :url "http://www.eclipse.org/legal/epl-v10.html"}
  :dependencies [[org.clojure/clojure "1.5.1"]
                 [org.clojure/core.typed "0.2.3"]]
  :plugins [[lein-typed "0.3.0"]] 
  ; 対象namespaceを指定
  :core.typed {:check [try-typed.core]})
```
静的型チェックを実行します。
```
$ lein typed check
Initializing core.typed ...
"Elapsed time: 11563.779405 msecs"
core.typed initialized.
Start collecting try-typed.core
Finished collecting try-typed.core
Collected 1 namespaces in 11658.922614 msecs
Start checking try-typed.core
Checked try-typed.core in 423.743596 msecs
Checked 1 namespaces (approx. 33 lines) in 12087.276047 msecs
```
`check`の替わりに`coverage`を指定すると、型指定annotationの網羅率を表示します。
```
lein typed coverage
Initializing core.typed ...
"Elapsed time: 3286.597969 msecs"
core.typed initialized.
Start collecting try-typed.core
Finished collecting try-typed.core
Collected 1 namespaces in 3383.256821 msecs
Checked 0 namespaces (approx. 0 lines) in 3385.801947 msecs
Found 4 annotated vars out of 4 vars
100% var annotation coverage
```
