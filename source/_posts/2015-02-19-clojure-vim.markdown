---
layout: post
title: "[Clojure] UbuntuでClojure & VIM開発環境"
date: 2015-02-19 12:23
comments: true
categories: 
---
手順の覚書

### Javaインストール
```
$ sudo apt-get install openjdk-7-jdk
```

### leiningenインストール
```
$ curl -O https://raw.github.com/technomancy/leiningen/stable/bin/lein  
$ mv lein ~/bin  
$ chmod 755 ~/bin/lein  
```

### vimインストール
```
$ sudo apt-get install vim
```

### vim関連ファイルを作っておく 
```
$ touch ~/.vimrc && mkdir ~/.vim && mkdir ~/.vim/bundle
```

### プラグインの導入を簡便にするために[pathogen](https://github.com/tpope/vim-pathogen)導入
```
$ mkdir -p ~/.vim/autoload ~/.vim/bundle && curl -LSso ~/.vim/autoload/pathogen.vim https://tpo.pe/pathogen.vim
```
`~/.vimrc`に以下追加
```
execute pathogen#infect()
syntax on
filetype plugin indent on
```

### [vim-clojure-static](https://github.com/guns/vim-clojure-static)インストール
```
$ cd ~/.vim/bundle && git clone git://github.com/guns/vim-clojure-static
```

### [fireplace.vim](https://github.com/tpope/vim-fireplace)インストール
```
$ cd ~/.vim/bundle && git clone git://github.com/tpope/vim-fireplace.git
```

### [paredit.vim](http://www.vim.org/scripts/script.php?script_id=3998)インストール
```
$ cd ~/.vim/bundle && git clone git://github.com/vim-scripts/paredit.vim
```

### 新規プロジェクトを作成して動作確認
```
$ lein new try-fireplace
```
プロジェクトディレクトリに移動しREPLを起動しておく
```
$ cd try-fireplace
$ lein repl
```
別のコンソールでプロジェクトディレクトリに移動し、ソースコードを開く
```
$ cd try-fireplace
$ vim vi src/try_fireplace/core.clj
```
サンプル関数をvim内で呼んでみる
```
(ns try-fireplace.core)

(defn foo
  "I don't do a whole lot."
    [x]
      (println x "Hello, World!"))

;; 以下の行を追加し、コマンド cpp で実行してみる
(foo "fireplace.vim")
```

正常にインストールされていれば、下部に標準出力と戻り値が表示される
```
fireplace.vim Hello, World!  
nil
```

### vimヘルプタグ生成
以下をvim内で実行し、プラグインのヘルプを生成しておく
```
:Helptags
```

以下でプラグインのヘルプを見ることができる
```
:help fireplace  
:help paredit
```

### 参考サイト
* [Clojure with Vim and fireplace.vim](http://clojure-doc.org/articles/tutorials/vim_fireplace.html)
* [Simple cheat sheet for vim fireplace and paredit](https://gist.github.com/nblumoe/5450099)
