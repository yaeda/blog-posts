---
title: React Native アプリ開発初期設定（2020/10 版）
date: 2020-10-09
---

Expo に Bare Workflow というのが出来てからなんとなく自分の中で React Native アプリ開発の初期設定の型が固まってきた感じがするので，一旦以下の項目について現状のやり方と次の機会に確認すべきポイントをまとめておく．

- プロジェクトの作成
- App Name / Package Name / Bundle ID の変更
- バージョン管理方法

なおこの構成で 3 つほどアプリを作ってきてはいるが，どれも実際にストアに出すには至っていない．商用アプリ向けにはもっといい方法があるかもしれない（知りたい）．

## TL;DR

Expo bare workflow + unimodules + Typescript なプロジェクトを作成

```sh
npm install -g expo-cli # if you don't have expo command
expo init --template expo-template-bare-typescript
```

アプリ名と id を変更 （+マニュアル修正）

```sh
npx react-native-rename "My App" -b io.github.yaeda.myapp
```

`react-native-version`でバージョン管理

```json title=package.json
"scripts": {
  "postversion": "react-native-version
}
```

## プロジェクトの作成

React Native でアプリを作るとなるとまず Expo が使えるかを検討する．Expo には managed workflow と bare workflow という二通りの使い方があり，managed workflow の方が使えれば話は早く，何も考えずに巻かれておけばよいと思う．ただし managed workflow には制約があり，一部の Native 機能が使えない．それぞれの workflow や制約については以下のリンクを参照．

- [Workflows \- Expo Documentation](https://docs.expo.io/introduction/managed-vs-bare/)
- [Limitations \- Expo Documentation](https://docs.expo.io/introduction/why-not-expo/#build-service-only-works-in-the-managed)

自分の場合は Bluetooth を使うことが多く，managed workflow が使えないので bare workflow を愛用している．bare workflow を使うと，普通の React Native アプリとして開発しながら unimodules という仕組みを利用して Expo 関連のライブラリを使えるようになる．完全に Expo から離れるという選択肢もあるとは思うが，経験上 Expo 関連のライブラリを使いたくなるケースが多く，後から対応するのはやや大変（後述）なので最初から bare workflow を使うと決めてかかったほうが幸せになれる．

ということで以下のコマンドで Expo bare workflow + unimodules + Typescript なプロジェクトを作成する．

```sh
npm install -g expo-cli # if you don't have expo command
expo init --template expo-template-bare-typescript
```

### 確認ポイント: 既存アプリのマイグレーションについて

既に出来上がっている React Native アプリに unimodules 対応を入れる場合は以下のページを参照．

[Installing react\-native\-unimodules \- Expo Documentation](https://docs.expo.io/bare/installing-unimodules/)

現状はマニュアルで書き換える必要のあるファイル・変更数が多く，つらい．今後マイグレーションスクリプトなどができる可能性があるのでタイミングをみて確認すると良さそう．

### 確認ポイント: `create-react-native-app`コマンドについて

また，上記の unimodules 導入ページでおすすめされている`npx create-react-native-app`というコマンドは実行後の選択肢で「Default new app」を選ぶと unimodules 入りの bare workflow のプロジェクト（非 typescript）が生成されるが，「Template from expo/examples」を選ぶともれなく managed workflow になるので注意．今後 template の追加などで改善されるかもしれないのでここも確認ポイント．

## App Name / Package Name / Bundle ID の変更

`expo init`コマンドで指定できるアプリ名には（空白が使えない等の）制約があったり，Package Name（Android）や Bundle ID（iOS）が自動で付与されるので変更する必要がある．

ということで以下のコマンドでアプリ名と id を変更する．

```sh
npx react-native-rename "My App" -b io.github.yaeda.myapp
```

が，対応不十分でかなりマニュアルでの修正が必要．みんなこれどうやってるのか教えて欲しい．

### 確認ポイント: `expo init`のオプション

`expo init`のオプションは以下のページで解説がある．

[Expo CLI \- Expo Documentation](https://docs.expo.io/workflow/expo-cli/#expo-init)

`--name`は「The name of your app visible on the home screen.」とあるが空白が入れられないなどの制約がある．昔はキャメルケースしかサポートしていなかったが，スネークケースやケバブケースも使えるようになったという経緯があるので今後さらなる改善があるかもしれない．

また，`--android-package`や`--ios-bundle-identifier`のようなオプションも検討された形跡がある．

- [expo init disregards \-\-android\-package and \-\-ios\-bundle\-identifier arguments · Issue \#824 · expo/expo\-cli](https://github.com/expo/expo-cli/issues/824)
- [feat\(expo\-cli\): add package and bundle identifier to init by byCedric · Pull Request \#1878 · expo/expo\-cli](https://github.com/expo/expo-cli/pull/1878)

このあたり公式でサポートされれば使いたいので要チェック．

### 確認ポイント: ライブラリのアップデート 

公式の方法がない場合は 3rd party ライブラリが存在しないかチェックする．現時点で有力な`react-native-rename`や`react-native-ci-tools`は開発が止まっている．

## バージョン管理方法

マルチプラットフォーム展開時のバージョン管理について，は今の所全てに共通のバージョンを付与する方針でやっている．pacakge.json の`postversion`に`react-native-version`を使うと楽．

```json title=package.json
"scripts": {
  "postversion": "react-native-version
}
```

こうすると`yarn version`すると連動して android と ios のバージョンも上げくれる．

ただし，app.json に expo フィールドが存在すると managed workflow だと勘違いされて正しくバージョン更新がされない問題がある．

- [Issue with ejected Expo Project · Issue \#105 · stovmascript/react\-native\-version](https://github.com/stovmascript/react-native-version/issues/105)

そのため，app.json から expo フィールドを削除するか，以下の Pull Request 版を使うと良い．

- [\-\-ignore\-expo cli option by killerchip · Pull Request \#150 · stovmascript/react\-native\-version](https://github.com/stovmascript/react-native-version/pull/150)

### 確認ポイント: Issue \#105

上で説明した問題が解決されているかを確認する．早く対応してほしいので，同意できる人はコメントするなり action 付けるなりして欲しい．

### 確認ポイント: OTA について

実は OTA を使ったことがないので，OTA を真面目にやるとバージョン周りで考えることが増える気がしている．

## 最後に

一旦まとめた．他にも CI とかも固まりつつあるのでこのあたりも今後まとめたい．
