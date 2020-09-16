---
layout: post
title: "Custom Domain with Github Pages"
date: 2012-12-12 00:43
comments: true
categories: github octopress
---

`yaeda.github.com`のままではあれなので，持っているドメインを設定してみた．
ここには技術的な話を書いていこうと思うので，`tech.kazeor.net`に設定．

Octopress側は[Document](http://octopress.org/docs/deploying/github/#custom_domains)通りにCNAMEファイルを作り，`git push`．


```
echo 'tech.kazeor.net' >> source/CNAME
```

VALU-DOMAIN側は以下のようにしてcnameを設定．最後のコンマを忘れずに．
```
cname tech pages.github.com.
```

この調子で今日からOctopressで一人Advent Calendarでもやるかな．
