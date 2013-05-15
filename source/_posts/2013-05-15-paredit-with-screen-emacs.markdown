---
layout: post
title: "screen内のEmacsでparedit.elを使う"
date: 2013-05-15 15:23
comments: true
categories: Clojure
---

Clojureなど、Lisp系言語の編集に便利な[paredit.el](http://emacswiki.org/emacs/ParEdit)ですが、[GNU screen](http://www.gnu.org/software/screen/)内で起動した[Emacs](http://www.gnu.org/software/emacs/)で利用しようとしたところ、Ctrl-矢印キーが使えず(5C, 5Dなどと入力される)、飲み込みや吐き出しの操作ができませんでした。

emacs起動時のコマンドでTERM変数を指定しておくと、とりあえず解決するようです。
```
TERM=xterm emacs -nw somefile
```

参考にしたURL
[http://lists.gnu.org/archive/html/screen-users/2009-12/msg00037.html](http://lists.gnu.org/archive/html/screen-users/2009-12/msg00037.html)


