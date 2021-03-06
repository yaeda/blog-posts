---
layout: post
title: "Better Full Screen Mode on Cocoa Emacs 24.3"
date: 2013-05-03 00:00
comments: true
categories: emacs osx
---

Ubuntu の Emacs を 24 系に上げて elsp の管理を全て ELPA(package.el)に任せ始めたので，Mac の方の Emacs も 24 系に上げることにした．現状の最新版 24.3 のインストール方法と OSX Lion Style でない fullscreen の設定について．

## Emacs 24.3 インストール

軽く調べると，emacs-24.3.tar.xz を落としてきて，インラインパッチを当てて手動でビルドしている例が多い．パッチをあてる理由を理解していないため，今回は普通に brew から入れる．問題に直面したら入れ直すことにする．

brew によるインストールは以下のコマンドだが，本家の git から落としてくるのでとても時間がかかる．お急ぎの場合はオプション外すのが良い(が，そっちでは下記の fullscreen の設定は試していない）

```sh
brew install emacs --cocoa --HEAD --use-git-head
```

以下のどちらかのコマンドで/Applications にインストールされる．

```sh
brew linkapps
ln -s /usr/local/Cellar/emacs/HEAD/Emacs.app /Applications
```

## Full Screen Mode の設定

23 系では ns-toggle-fullscreen があり top bar も消してフルスクリーン化してくれるので最高だった． 24 系では ns-toggle-fullscreen は無くなり，OSX Lion Style のフルスクリーン(toggle-frame-fullscreen)が標準になってしまっている． toggle-frame-maximized というのもあるが，top bar は残ったままだし，maximize が控えめで画面いっぱいに広がってくれない（全然 maximize じゃない！）．

「改悪じゃねーか」と思っていたが，ns-use-native-fullscreen で toggle-frame-fullscreen の挙動を変えられた．

init.el 辺りに以下を書いておくと，toggle-frame-fullscreen が 23 系の ns-toggle-fullscreen と同じ挙動をしてくれて幸せ Emacs 生活が再始動する．

```
(setq ns-use-native-fullscreen nil)
```

右上の矢印も消えてくれる．
