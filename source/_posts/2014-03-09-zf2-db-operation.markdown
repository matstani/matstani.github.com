---
layout: post
title: "[ZF2] データベース操作色々"
date: 2014-03-09 17:23
comments: true
categories: PHP ZF2 ZendFramework2
---
ZF2の提供するTableGatewayを利用した基本的なCRUD手順は[チュートリアル](http://framework.zend.com/manual/2.2/en/user-guide/database-and-models.html)で紹介されています。
ここでは、より複雑な例をいくつか紹介します。

## 検索条件指定
TableGatewayを利用した検索で条件を指定するには、`select`メソッドに無名関数を渡します。  
[サンプルコード](https://gist.github.com/matstani/9445201)
{% gist 9445201  %}

## 結果を配列で取得
TableGatewayを利用した検索では、結果はEntityオブジェクトに変換された状態で取得できますが、JOINしたカラム等は失われてしまいます。  
全てのカラムを取得したい場合は、以下のように`select`オブジェクトを生成して実行します。  
[サンプルコード](https://gist.github.com/matstani/9445275)
{% gist 9445275 %}

## JOIN
JOINする場合のサンプルコードです。  
[サンプルコード](https://gist.github.com/matstani/9445347)
{% gist 9445347 %}

## LEFT JOIN, RIGHT JOIN, INNER JOIN
結合方法を指定する場合は、`join`メソッドの第4引数で指定します。  
[サンプルコード](https://gist.github.com/matstani/9445475)
{% gist 9445475 %}

## 集計
GROUP BY を使用した集計のサンプルコードです。  
[サンプルコード](https://gist.github.com/matstani/9445604)
{% gist 9445604 %}

## 副問い合わせ
`select`オブジェクトを組み合わせることで、副問い合わせを行うことができます。
WHERE句に副問い合わせを指定する例です。  
[サンプルコード](https://gist.github.com/matstani/9445934)
{% gist 9445934 %}

JOIN句に副問い合わせを指定する例です。  
[サンプルコード](https://gist.github.com/matstani/9445831)
{% gist 9445831 %}
