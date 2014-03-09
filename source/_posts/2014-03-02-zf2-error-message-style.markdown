---
layout: post
title: "[ZF2] バリデーションメッセージのスタイルを変更する"
date: 2014-03-02 19:06
comments: true
categories: PHP ZF2 ZendFramework2
---
Zend Framework2でフォームバリデーションを行った際のエラーメッセージには、特定のスタイル(class)が指定されていないため、メッセージを赤字で表示したい、といった場合に困ります。

デフォルトの表示方法は、[ビューヘルパー](http://framework.zend.com/manual/2.2/en/modules/zend.form.view.helpers.html)`formElementErrors`として登録されています。  
これを自作のクラスで置き換えることで、任意のスタイルでエラーメッセージを表示させることができます。

例として、`Album\Helper\FormElementErrors`を作成します。  
[FormElementErrors.php](https://gist.github.com/matstani/9304648)
{% gist 9304648 FormElementErrors.php  %}
上記の例ではエラーメッセージの`ul`要素に`class="err-msg"`を指定しています。

作成したビューヘルパーは、`Module.php`の`getViewHelperConfig`メソッドで登録します。  
[Module.php](https://gist.github.com/matstani/9304648)
{% gist 9304648 Module.php  %}

登録後、バリデーションメッセージが指定したスタイルで表示されるようになります。
