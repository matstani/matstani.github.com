---
layout: post
title: "[Clojure] tools.namespaceを利用したREPL上でのソースコードリロード"
date: 2014-08-05 16:42
comments: true
categories: Clojure
---
JVMを再起動することなく、プロジェクト全体のソースコードのリロードを実現するツール[tools.namespace](https://github.com/clojure/tools.namespace)を紹介します。  

ClojureにはREPLが付属しており、動作を確かめながらソフトウェアを開発することができます。  
修正後のソースコードをREPL上に読み込むには通常、  
`(use 'namespace :reload または :reload-all)`  
を利用します。例)
```
user=> (use 'my.app :reload-all)
```
しかし、`:reload`、`reload-all`を利用したリロードでは、

* ソースコードから削除した定義がJVM上に残ってしまう
* 依存するファイルのリロード漏れがあると、REPL上で古いコードと新しいコードが混在してしまう

といった不便な点があります。  
リロードがうまくいかない場合は、JVMごと再起動するのが常套手段ですが、プロジェクトが大きくなるほど時間がかかります。  
`tools.namespace`を利用するとJVMを再起動することなく、プロジェクト全体のソースコードのリロードを実現できます。

## `tools.namespace`の使い方
`project.clj`の`dependencies`に以下を追記します。
```
[org.clojure/tools.namespace "0.2.5"]
```
開発中にソースコードを変更した際は、以下をREPL上で実行することで、ソースコードのリロードが行われます。
```
user=> (use '[clojure.tools.namespace.repl :only [refresh]])
user=> (refresh)
```

## リロードを活用するために
`tools.namespace`によるリロードをスムーズに行うためには、ソフトウェアが以下のルールに従っている必要があります。

### 1. グローバルな状態を持たない
動作中に変化するアプリケーションの状態をグローバルVarに格納している場合、`(refresh)`は状態をクリアしてしまいます。  
リロードをスムーズに行うためには、アプリケーションの動作中にグローバルVarの状態を参照しない設計(状態を表すデータ構造はパラメータとして必要な関数に渡す設計)が求められます。  


### 2. ライフサイクルを管理できる
REPL上からアプリケーションの起動、停止を行う関数を用意しておくことが求められます。  
起動関数では、アプリケーションの動作に必要なリソースを確保し、内部状態を表すデータ構造に組み立てて、必要な関数に渡します。  
終了関数では、全てのリソースを開放し、まっさらな状態に戻します。  
これらの関数と`tools.namespace`利用した開発の流れは以下のようになります。

Step 1. REPL起動  
Step 2. アプリケーション起動
```
user=> (use 'clojure.tools.namespace.repl)
user=> (refresh)
user=> (def my-app {:state-of-world (ref {})
                    :object-handle (atom nil)})
user=> (start my-app)
```
Step 3. 動作確認  
Step 4. ソースコード修正  
Step 5. アプリケーション再起動
```
user=> (stop my-app)
user=> (refresh)
user=> (def my-app {:state-of-world (ref {})
                    :object-handle (atom nil)})
user=> (start my-app)
```
※上記でグローバルVar`user/my-app`を利用していますが、アプリケーション内からは直接`user/my-app`を参照せず、ローカルパラメータとしてのみ参照されるため、リロードによって不都合が発生することはありません。

## 状態管理のフレームワーク
リロード可能なアプリケーションを実現するために、アプリケーションのライフサイクル管理の統一的なインターフェースを定義し、内部状態を適切に管理するための仕組みとして、[Componentフレームワーク](https://github.com/stuartsierra/component)が`tools.namespace`と同じ作者によって提供されています。

## 注意点
* [AOTコンパイル](http://clojure.org/compilation)非対応
* 一部のライブラリ([ring-devel](https://github.com/ring-clojure/ring/tree/master/ring-devel)など)は機能が衝突する
* `(reresh)`を実行した名前空間はクリアされる(REPL上で定義した内容は削除される)


## 参考
* [tools.namespace](https://github.com/clojure/tools.namespace)
* [stuartsierra.component](https://github.com/stuartsierra/component)
* [My Clojure Workflow, Reloaded](http://thinkrelevance.com/blog/2013/06/04/clojure-workflow-reloaded)
