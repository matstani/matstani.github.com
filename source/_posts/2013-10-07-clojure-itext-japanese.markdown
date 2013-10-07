---
layout: post
title: "[Clojure] iTextでPDFファイル作成"
date: 2013-10-07 17:16
comments: true
categories: Clojure
---
ClojureでPDFファイルを作成するためのライブラリとして[clj-pdf](https://github.com/yogthos/clj-pdf)がありますが、そのままでは日本語フォントに対応していないようなので、直接[iText](http://itextpdf.com/)を使う手順を紹介します。

## テスト用プロジェクト作成
```
$ lein new app try-itext
$ cd try-itext
```
`project.clj`の`:dependencies`に`[com.itextpdf/itextpdf "5.4.4"]` `[com.itextpdf/itext-asian "5.2.0"]`を追記します。
```
$ lein deps
```

## サンプルコード
[core.clj](https://gist.github.com/matstani/6864398)
{% gist 6864398 %}

## プログラム実行
```
$ lein run
```
実行すると、`test.pdf`が作成されます。
