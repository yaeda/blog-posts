---
title: 他のリポジトリのイベントで GitHub Actions をトリガーする
date: 2020-10-06 22:40
---

別のリポジトリのイベントをきっかけとして GitHub Actions の workflow をトリガーするというのをこのブログで設定したのでまとめておく．

このブログのソースコードは以下の２つに分かれている．

- Markdown の文章データを管理するリポジトリ（[yaeda/blog-posts](https://github.com/yaeda/blog-posts)）
- サイトのソースコードを管理するリポジトリ（[yaeda/yaeda\.github\.com](https://github.com/yaeda/yaeda.github.com)）

今回，yaeda/blog-posts の方に記事を上げたら，yaeda/yaeda.github.com の方で Build & Deploy を行うようにした．

## yaeda/blog-posts 側の設定

以下の yml コードが 「main ブランチの posts ディレクトリの Markdown ファイルに変更があったら yaeda/yaeda.github.com に`deploy`というイベントを投げる」という設定．GitHub Actions の [repository_dispatch](https://docs.github.com/en/free-pro-team@latest/actions/reference/events-that-trigger-workflows#repository_dispatch) という機能を使っているが， [workflow_dispatch](https://docs.github.com/en/free-pro-team@latest/actions/reference/events-that-trigger-workflows#workflow_dispatch) でも同様のことができると思う．どちらも GitHub API 経由で workflow をトリガーする機能で，これを別のリポジトリの workflow の中で利用する．

```yml title="blog-posts/.github/workflows/dispatch.yml"
name: dispatch

on:
  push:
    branches:
      - main
    paths:
      - "posts/**.md"

jobs:
  dispatch:
    runs-on: ubuntu-latest
    steps:
      - name: Dispatch
        uses: peter-evans/repository-dispatch@v1
        with:
          token: ${{ secrets.REPO_ACCESS_TOKEN }}
          repository: yaeda/yaeda.github.com
          event-type: deploy
```

API を使う部分は [peter\-evans/repository\-dispatch](https://github.com/peter-evans/repository-dispatch) という Action を作ってくれている人がいたので利用している．権限が足りずおなじみの `${{ secrets.GITHUB_TOKEN }}` が使えないので personal access token を用意する必要がある点に注意．詳しくはこちら（[token on peter\-evans/repository\-dispatch](https://github.com/peter-evans/repository-dispatch#token)）．

## yaeda/yaeda.github.com の設定

こちらでは `deploy` というイベントを受け取って Build & Deploy を行う．以下の例では「 main ブランチへの変更 もしくは `deploy` イベント」をトリガーとして publish ジョブが走るようになっている．ジョブの中で yaeda/yaeda.github.com と yaeda/blog-posts の２つのリポジトリを checkout してきて Build & Deploy を行っている．完全なコードはこちら（[yaeda\.github\.com/deploy\.yml at main](https://github.com/yaeda/yaeda.github.com/blob/main/.github/workflows/deploy.yml)）．

```yml title="yaeda.github.com/.github/workflows/deploy.yml"
name: deploy

on:
  push:
    branches:
      - main
  repository_dispatch:
    types:
      - deploy

jobs:
  publish:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v2

      - name: Checkout Blog Posts
        uses: actions/checkout@v2
        with:
          repository: yaeda/blog-posts
          path: blog-posts

      .
      .
      .
```

## 所感

ここで紹介した方法は連携する両方のリポジトリに設定が必要なので，自分のリポジトリや所属している組織のリポジトリのような閉じた状況でないと使いづらい．たとえば「React Native の新しいバージョンがリリースされたら更新する PR つくってテストしよう」みたいなのは難しい．そういうときは Renovate とか使いましょう．

最近はもう GitHub Actions しか使ってなくて，機能も十分だと感じている．あまり凝ったことをしてないってものあるけど，便利．他の人の技も盗みたいと思って zenn.dev を検索してみたら同じ内容の記事を見つけた．こちらも参考にできそう．

- [検索 \| Zenn](https://zenn.dev/search?q=github%2520actions)
- [Github Actions で他のリポジトリからの変更通知を受け取って PR を作成する Workflow \| Zenn](https://zenn.dev/mizchi/articles/3117b92a834531361fc8)
