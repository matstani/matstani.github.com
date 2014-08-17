---
layout: post
title: "[Clojure] componentフレームワークを利用した開発"
date: 2014-08-18 03:48
comments: true
categories: 
---
[componentフレームワーク](https://github.com/stuartsierra/component)は動作中に状態を保持するソフトウェアに関して

* 起動・停止のインターフェイスを定義する
* ソフトウェアをコンポーネントに分割し、コンポーネント間の依存関係を宣言的に記述する
* 動作中のソフトウェア全体の状態を一つのデータ構造（システム）にまとめる

といった機能を持つフレームワークです。componentフレームワークを利用することで、[tools.namespaceを利用した開発](/blog/2014/08/05/clojure-tools-dot-namespace/)が容易になります。  

ring/compojureベースのWebアプリケーションに、componentフレームワークを適用する例を紹介します。

### プロジェクト作成
compojure用テンプレートを利用してプロジェクトを作成します。
```
$ lein new compojure example-component-ring
```

### 依存ライブラリ追加
`project.clj`の`:dependencies`に以下を追記します。
#### componentフレームワーク
```
[com.stuartsierra/component "0.2.1"]
```
#### tools.namespace
```
[org.clojure/tools.namespace "0.2.5"]
```
#### [ring-jetty-adapter](https://github.com/ring-clojure/ring#libraries)(ringアプリケーションの直接起動、停止)
```
[ring/ring-jetty-adapter "1.3.0"]
```

### HttpServerコンポーネント定義
`example-component-ring.component`ネームスペースに、HttpServerコンポーネントを記述した例です。  
[component.clj](https://gist.github.com/matstani/1ad5129577c065d4a8cf#file-component-clj)
<script src="https://gist.github.com/matstani/1ad5129577c065d4a8cf.js"></script>
コンポーネントは`Lifecycle`プロトコルを実装したレコードとして定義します。  
`Lifecycle`プロトコルはメソッドとして`start`と`stop`を持っています。これをHTTPサーバの起動、停止を行うように実装しています。この時、サーバオブジェクト（このアプリケーションにおける状態）をレコードのフィールドに格納しています。  


`create-system`関数は、コンポーネントを作成し、１つのシステムにまとめる関数です。内部で`system-map`関数を利用しています。上記では、HttpServerコンポーネントのみ作成していますが、複数のコンポーネントを組み合わせて１つのシステムにまとめることや、コンポーネント間の依存関係を記述することができます。

### システムの起動・停止・リロード
システムの起動・停止・リロードを行うための関数を定義します。REPLから利用しやすいように、`user`ネームスペースに定義する例を紹介します。
#### project.clj追記
起動・停止・リロード用の関数は、開発時のみ読み込まれるファイルに記述します。  
`project.clj`の`:profiles :dev`に以下を追記します。
```
:source-paths ["dev"]
```
`project.clj`全体は以下のようになります。
```
(defproject example-component-ring "0.1.0-SNAPSHOT"
  :description "FIXME: write description"
  :url "http://example.com/FIXME"
  :dependencies [[org.clojure/clojure "1.6.0"]
                 [compojure "1.1.8"]
                 [com.stuartsierra/component "0.2.1"]
                 [ring/ring-jetty-adapter "1.3.0"]
                 [org.clojure/tools.namespace "0.2.5"]]
  :plugins [[lein-ring "0.8.11"]]
  :ring {:handler example-component-ring.handler/app}
  :profiles
  {:dev {:dependencies [[javax.servlet/servlet-api "2.5"]
                        [ring-mock "0.1.5"]]
         :source-paths ["dev"]}})

```
#### user.clj作成
`dev/user.clj`ファイルを作成し、システム起動・停止・リロードを行う関数を定義します。
[user.clj](https://gist.github.com/matstani/4852df3eb4856bccbce4#file-user-clj)
<script src="https://gist.github.com/matstani/4852df3eb4856bccbce4.js"></script>
プロジェクト内でREPLを起動すると自動的にこれらの関数が読み込まれた状態になります。
#### システムを起動する
```
user> (go)
;; Starting HTTP server
#com.stuartsierra.component.SystemMap{:http-server #example_component_ring.component.HttpServer{:port 3000, :server #<Server org.eclipse.jetty.server.Server@120f5c9>}}
```
#### システムを停止する
```
user> (stop)
;; HTTP server stopped
#com.stuartsierra.component.SystemMap{:http-server #example_component_ring.component.HttpServer{:port 3000, :server nil}}
```
#### （ソースコード修正時）システムをリロードする
```
user> (reset)
:reloading (example-component-ring.handler example-component-ring.component example-component-ring.test.handler user)
;; Starting HTTP server
#com.stuartsierra.component.SystemMap{:http-server #example_component_ring.component.HttpServer{:port 3000, :server #<Server org.eclipse.jetty.server.Server@1fc6f6a>}}
```

### 複数のコンポーネントの連携
componentフレームワークでは、複数のコンポーネントの依存関係を記述し、コンポーネント同士を連携させることができます。  
ここでは、Databaseコンポーネントを導入し、HttpServerコンポーネントが利用する（HttpServerコンポーネントがDatabaseコンポーネントに依存する）例を紹介します。
#### [MongoDB](http://www.mongodb.org/)導入
Databaseコンポーネントの中身として、MongoDBを利用することにします。ClojureからMongoDBを利用するためのクライアントライブラリとして[Monger](http://clojuremongodb.info/)を利用します。`project.clj`に以下を追記します。
```
[com.novemberain/monger "2.0.0"]
```
#### Databaseコンポーネント定義
`component.clj`にDatabaseコンポーネントを定義します。  
[component.clj](https://gist.github.com/matstani/d46e1929d1419547ed5c#file-component-clj)
<script src="https://gist.github.com/matstani/d46e1929d1419547ed5c.js"></script>
`HttpServer`と同様に、`Lifecycle`プロトコルを実装したレコードとして定義しています。  
既存コードへの変更点として、`HttpServer`レコードに、`database`フィールドを追加しています。このフィールドに`Database`コンポーネントのオブジェクトを、状態として保持します。  
また、`create-system`関数では、`using`関数を利用して依存関係を定義しています。上記では、`HttpServer`コンポーネントが`Database`コンポーネントを利用する、という関係を記述しています。これにより`HttpServer`インスタンスの`database`フィールドには、`Database`インスタンスが代入されます（依存関係の定義に従って代入が自動的に行われる仕組みをDependency Injectionといいます）。

#### コンポーネントをringハンドラに渡す
上記までの例で、HttpServerコンポーネントにDatabaseコンポーネントを渡すことはできていますが、ringアプリケーションのエントリポイントとなるハンドラに、直接引数として渡すことはできないため、Webアプリケーション内からDatabaseコンポーネントを利用するためには、さらに一工夫が必要になります。  
例として、requestマップ内にDatabaseコンポーネントを追加するringミドルウェアを利用する方法を紹介します。  
[component.clj](https://gist.github.com/matstani/1fb4b7b94a71780a423b#file-component-clj)
<script src="https://gist.github.com/matstani/1fb4b7b94a71780a423b.js"></script>
`wrap-app-component`ミドルウェアで、requestに`Database`コンポーネントを追加しています。これにより、ringハンドラ内の処理で、Databaseコンポーネントを利用できるようになります。  
[handler.clj](https://gist.github.com/matstani/4846ff717e4997139937#file-handler-clj)
<script src="https://gist.github.com/matstani/4846ff717e4997139937.js"></script>

### 参考リンク
* [video from Clojure/West 2014](https://www.youtube.com/watch?v=13cmHf_kt-Q)
* [tools.namespace](https://github.com/clojure/tools.namespace)
* [stuartsierra.component](https://github.com/stuartsierra/component)
* [My Clojure Workflow, Reloaded](http://thinkrelevance.com/blog/2013/06/04/clojure-workflow-reloaded)
