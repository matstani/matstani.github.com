---
layout: post
title: "S3バケット内のファイルをファイル名のパターンで削除"
date: 2015-04-07 11:31
comments: true
categories: 
---
`my.bucket.name`バケット内の`temp`をファイル名に含むファイルを削除
```
$ aws s3 rm --recursive s3://my.bucket.name/ --exclude '*' --include '*temp*'
```
実行前に`--dryrun`で確認しておく
