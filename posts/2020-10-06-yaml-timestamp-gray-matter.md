---
title: YAML の Timestamp と gray-matter の挙動
date: 2020-10-06
---

YAML の Timestamp 周りでややハマった.

このブログは Markdown で書いていて，先頭に以下のようなデータを front matter として付与している．

```yml
title: ライフログ
date: 2020-10-02 21:30
```

この`date`をシンプルにしたいと思った．

一日に複数のポストがあったとき，順序を決定するために`date`に「時分」を入れているが，書き始め・書き終わり・投稿で時間差があるため，どの時間に設定するか悩むときがある．時間をそこまで厳密に管理する必要もないし，一日に一回しかポストしないケースの方が大半なのでシンプルに「年月日」にしたほうが楽．

ということで試しに`date: 2020-10-04`としてみたらソートに失敗した．原因は [gray\-matter](https://www.npmjs.com/package/gray-matter) のパース結果の型に以下のような違いがあったため．

- `date: 2020-10-02 21:30:00` => Date オブジェクト
- `date: 2020-10-02 21:30` => string
- `date: 2020-10-02` => Date オブジェクト
- `date: "2020-10-02"` => string

YAML はデータ型に Timestamp という型があって，gray-mattter は Timestamp として正しいフォーマットの時は Date オブジェクトを返し，そうでないときは string を返すようになっている様子．

[Language\-Independent Types for YAML™ Version 1\.1](https://yaml.org/type/)

型を揃えたいが front matter の書き方に制約を加えるのは，書き手が人間であることを考えると良くない仕様と思われるので，gray matter の変換結果を以下のようにすべて string に変換するようにした．

```ts
// Use gray-matter to parse the post metadata section
const matterResult = matter(fileContents);
if (
  typeof matterResult.data.date === "object" &&
  typeof matterResult.data.date.toISOString === "function"
) {
  matterResult.data.date = matterResult.data.date.toISOString();
}
```

雑だがとりあえずはいいだろう．もっとも，ファイル名にも日付を入れていて重複を感じているので front matter の運用自体を変更するかもしれない．

---

このポスト書くの主に以下の２つの理由で非常に時間がかかった．

- 書き始めたら脳内が整理されて，検証が不十分な点が見えてきて再調査
- 簡潔でわかりやすい説明や順序を推敲

続けていくと良い訓練になりそう．
