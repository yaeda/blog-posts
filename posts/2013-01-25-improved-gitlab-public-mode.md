---
layout: post
title: "Improved GitLab Public Mode"
date: 2013-01-25 00:00
comments: true
categories: git gitlab
---

先日 GitLab の 4.1 が Release され，ついに Public Mode が導入されました．LDAP や同じく 4.1 から追加された Sign-up Page を合わせて使えば，GitHub っぽいものを Internal に作れるようになりました．

と思って 4.1 にアップグレードしたのですが，Public Mode と言っても，レポジトリのタイトルと clone 用の url がリスト表示されるだけでした．ソースコードやコミットログを見ることはできません．

そこで，最近 Rails を勉強し始めたので，腕試しに以下の機能拡張をしてみました．

- dashboard に”Public”というナビゲーションタブを追加
- Public なレポジトリには Reporter 相当の権限で誰でもアクセス可能

こんな感じでタブが追加されます．

![Screen Shot 2013-01-25 at 1.04.45](https://farm9.staticflickr.com/8192/8410694643_57780f3b07_z.jpg)

ソースコードは以下にあります．

[yaeda/gitlabhq at improve-public-mode · GitHub](https://github.com/yaeda/gitlabhq/tree/improve-public-mode)

GitLab のコードは規模が大きいですが，かなり読みやすいです．綺麗なコードだったので，修正も少なくすみました．なかなか参考になります．

自分で手を入れ始めると可愛くなりますね．LET’S HAPPY GITLAB LIFE!

[GitLab 4.1 - GITLAB Blog](http://blog.gitlab.org/gitlab-4-1-released/%20GitLab%204.1%20-%20GITLAB%20Blog)
