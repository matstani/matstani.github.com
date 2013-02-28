---
layout: post
title: "Octopressのスタイルシートを変更"
date: 2013-02-28 10:03
comments: true
categories: Octopress
---
[ドキュメント](http://octopress.org/docs/blogging/code/)にあるとおり、Octopressでは、ソースコードを簡単に貼り付けできます。
[gist](https://gist.github.com/)のコードを埋め込むには以下のように記述します。
```
{{ "{% gist gist_id [filename] " }}%}
```
このgistの埋め込み結果なのですが、Octopressのデフォルトのスタイルだと、テーブルの枠幅がでこぼこになって見た目が悪かったので、スタイルシートを微調整しました。
sassディレクトリ以下のscssファイルを修正すると、サイトに反映されるようです。

今回はsass/custom/_styles.scssに以下を追記しました。
```
.gist .line-data { width: 100% }
```

近いうちに本体でも修正されると思います。