---
layout: post
title: "[ZF2] フォームバリデーションメッセージを変更する"
date: 2014-03-02 18:24
comments: true
categories: PHP ZF2 ZendFramework2
---
Zend Framework2でフォームのバリデーションのエラーメッセージを変更するにはいくつか方法がありますが、InputFilter作成時のオプションで指定する方法を紹介します。

[ZF2チュートリアル](http://framework.zend.com/manual/2.2/en/user-guide/forms-and-actions.html)では、空欄チェックと、文字列の長さチェックが付加されていますが、これを日本語メッセージにしてみます。

[サンプルコード](https://gist.github.com/matstani/9303960)

{% gist 9303960 %}

冗長に見えますが、配列の組み合わせなので、関数化して再利用するのは容易です。  
関数化した例をいくつか掲載します。

[サンプルコード](https://gist.github.com/matstani/9304210)

{% gist 9304210 %}
