---
title: コードに Syntax Highlight とタイトルを付けた
date: 2020-10-14
---

このブログで markdown のレンダリングに`dangerouslySetInnerHTML`を使っていた部分を修正するついでにコードの Syntax Highlight に対応したりタイトルを付けられるようにした．

![](../images/2020-10-14-code-before-after.png)

改修に取り掛かるにあたって unified/remark/rehype/micromark などなど unified 関連を色々調べたがとりあえずまるっと全部入りの `remark-react` を使うことにした．Syntax Highlight は prism(refractor)系と highlight.js(lowlight)系があるみたいで違いがよく分からずいろいろ試して`react-syntax-highlighter` に落ち着いた．

以下のように書くことでタイトルを付与するようにしている．

````md
```lang:title
code
```
````

HTML 的には`figure`と`figcaption`を使っている．あまり見ない気がするので正しいのかわからないが，意味的には`img`とかと同じ扱いで良いと思っている．

```html:こんな感じ
<figure>
  <figcaption>title</figcaption>
  <pre>
    <code>
      code
    </code>
  </pre>
</figure>
```
