---
layout: post
title: "How to Organize Repositories Cloned From GitHub"
date: 2013-02-10 00:00
comments: true
categories: git github
---

GitHub で見つけたレポジトリはいつも~/dev/github に clone しているのですが，最近雑然として扱いにくっくなっていました．

ユーザー毎に~/dev/github/<username>/<repo>と分けてみたり，よくアクセスするものは~/dev/github/<repo>と直下に置いてみたりしていて，かなり見づらくなっていました．特にユーザー名とレポジトリ名がごっちゃで見づらい．

そんなことに悩んでいるときに，丁度良いツールを見つけました．

[GitHub のリポジトリを管理する小さいツール ghq - NaN days - subtech](http://subtech.g.hatena.ne.jp/motemen/20130207/1360210479)

基本はユーザー毎に~/dev/github/@<username>/<repo>に clone して，直下にはシンボリックリンクを作成．ユーザ名に@が付いているので見やすい．これはいい！

_以下蛇足_

どうせなら [hub コマンド](https://github.com/defunkt/hub)みたいに

```sh
ghq [-clone] yaeda/ghq
```

という感じで clone できれば楽だなと思い，試しに実装してみたのですが

```sh
ghq [-cd] @yaeda/ghq
```

と混乱しそうで微妙です．やめたほうがいいかも．
