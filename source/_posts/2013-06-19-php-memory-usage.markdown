---
layout: post
title: "[PHP]memory_get_usageのreal_usageオプションについて"
date: 2013-06-19 16:27
comments: true
categories: PHP
---
PHPで割り当てメモリ量を取得するには、[memory_get_usage](http://php.net/manual/ja/function.memory-get-usage.php)関数を利用できます。

この関数には、パラメータとして$real_usageを指定できるようになっており、

> これを TRUE に設定すると、システムが割り当てた実際のメモリの大きさを取得します。 省略したり FALSE を設定したりすると、 emalloc() が使用するメモリのみを報告します。 

とあるのですが、「システムが割り当てた実際のメモリの大きさ」と、「emalloc()が使用するメモリ」の違いがよくわかりません。

検索してみたところ、以下を見つけました。

[http://stackoverflow.com/questions/2290611/tracking-memory-usage-in-php](http://stackoverflow.com/questions/2290611/tracking-memory-usage-in-php)

回答によると、PHPのメモリマネージャは、メモリを確保する際、アプリケーションが必要とするメモリをその都度mallocするのではなく、より大きな単位で確保しておいて内部で管理しているそうです。(デフォルトで256KB単位。環境変数ZEND_MM_SEG_SIZEで設定)

ということで、割り当てメモリ量として以下2種類があることになります。

1. PHPのメモリマネージャが確保しているメモリ量($real_usage=true)
2. アプリケーションが実際に使用しているメモリ量($real_usage=false)

php.ini等で指定するmemory_limitで制限しているのは1.になります。  
環境変数 USE_ZEND_ALLOC=0 を設定すると、上記の仕組みが無効になり、必要量をその都度mallocする動作になるとのことです。

ちなみに、PHPの割り当てメモリ量を調べる関数としては、[memory_get_usage](http://php.net/manual/ja/function.memory-get-usage.php)の他に、[memory_get_peak_usage](http://php.net/manual/ja/function.memory-get-peak-usage.php)があり、こちらは呼び出し時点までに割り当てられた最大メモリ量を取得できます。
