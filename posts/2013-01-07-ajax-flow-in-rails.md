---
layout: post
title: "Ajax Flow in Rails"
date: 2013-01-07 00:00
comments: true
categories: rails ruby
---

ドットインストールのチュートリアルでブログシステムを作っているときに，ブログポストの削除リンクを以下のコードで実装した．

```ruby
<%= link_to 'Delete', post, :confirm => 'Sure?', :method => :delete, :remote => true %>
```

:confirm は確認用ダイアログのテキスト，:method はアクセスする HTTP メソッド，:remote は ajax アクセスするかしないか，の設定．

これは次のように展開される．

```html
<a href="/posts/1" data-confirm="Sure?" data-method="delete" rel="nofollow">Delete</a>
```

これだけではクリックしたら/post/1 に HTTP DELETE でアクセスするようにはならない．疑問が残ったので調べてみると，jquery_ujs.js というスクリプトで色々やっていた．

jquery_ujs.js は Gemfile で jquery-rails を設定していると直接読み込まれるようで，rails アプリケーションのディレクトリに見つけることは出来ない．私の環境だと~/.rbenv/versions/1.9.3-p194/lib/ruby/gems/1.9.1/gems/jquery-rails-2.1.4/vendor/assets/javascripts/jquery_ujs.js にあったが，Github のレポジトリ(rails/jquery-ujs · GitHub)か Developer Tools などで直接中身をみるのが早い．何故か圧縮されていないので簡単にコードを追うことができる．(因みに jquery 本体も圧縮されていない）

data-method については jquery_ujs.js の 185 行目の handleMethod 以下で，data-method に合わせて form を作って submit しているのが分かる．

ちなみに，jquery_ujs.js の 149 行目からの option の設定で，ajax が成功したときに ajax:success が trigger されることが分かる．ここまできてようやく以下のようにして結果を取得できることが分かる．(ここでは削除に成功した項目を fadeOut で消している)

```js
$('a[data-method="delete"]').on("ajax:success", function (e, data, status, xhr) {
  $("#post_" + data.post.id).fadeOut("slow");
});
```

古いバージョンはちょっと話が違うようだが，Rails 3.2 では jquery が標準で利用できるようになっていて，jquery_ujs というライブラリで ajax 周りの実装が簡単に行うことができるようになっているのが分かった．Web Framework ってすげー．
