---
title: "Git Flow ブランチモデルを自分なりにアレンジしてみた (at 1人開発)"
emoji: "🌲"
type: "idea"
topics:
  - "git"
  - "gitflow"
published: true
published_at: "2023-05-07 12:03"
---

## この記事は？

1人開発をしていたとき、自分流のブランチモデルができたのでここに記すことにしました。やっていた開発について、詳細はこちらの記事にまとめました。

@[card](https://zenn.dev/akuad/articles/team-like-solo-dev)

## 以前のソース管理

Git でのソース管理はやったことがありましたが、全部 main に直書きするだけでした。1人開発で、バックアップとっておく程度の心持ちだったので支障は出ませんでしたが。

ただ、「作業完了済みで動く版を常に保持しておいた方がいいのでは？」という思いもあって、main 直書き運用を改めるのを考えてみた次第です。

## できあがったブランチモデル

| ブランチ名 | 内容 | 分岐元 / マージ先 |
| --- | --- | --- |
| `main` (旧 `master`) | 実装に区切りがついたらここへ | - |
| `develop` | 各実装の統合と手動チェック | `main` |
| `feature/*` | 機能実装 | `develop` |
| `fix/*`| 細かな修正 | `develop` |
| `refact/*` | リファクタリング | `develop` |
| `github/*` | GitHub 関係のファイルの編集 | `main` |

※ `main` `develop` は Pull Request (PR) 経由のマージのみ, 他は作業場

※ `/*` には作業内容を示す短い単語列

Git Flow ブランチモデルをベースに、次のように手を加えました。(Git Flow の解説は既にたくさんあるので省略。)

* `feature` ブランチを `feature` `fix` `refact` に分割
* `github` ブランチを追加
* `release` ブランチは除去
  * リリース準備が必要なほど大規模な開発でなかったので

### `feature` と `fix` の違い

どちらとも機能実装の作業場です。2者の違いは、「新しい機能が増えるか、今ある機能が変わるか」です。

* `feature`: 新しくモジュールを作った, メソッドを追加した など
* `fix`: 現状ではメインに実装し辛いから引数のとり方を変えた など

### `refact`

本当なら `feature` 上でリファクタリングを完璧に済ませてしまうのが理想です。が、僕は一発で完璧なコードを書けるエリートではありません。後になって、もっと便利な関数 (メソッド) に気付くこともしばしばあります。

(皆さんも経験あるのでは？丹精込めて作ったけど、同じのが標準ライブラリに最初からあった、みたいな。)

`feature` `fix` をマージした後、もっと良い書き方を思いついたとき、その修正作業を行うのがこのブランチです。

### `github`

ブランチ例: `github/issue-temp`, `github/pr-temp`

GitHub では、指定のファイルを作成することで、Issue / PR のテンプレートをセットすることができます。

@[card](https://docs.github.com/ja/communities/using-templates-to-encourage-useful-issues-and-pull-requests/configuring-issue-templates-for-your-repository)

@[card](https://docs.github.com/ja/communities/using-templates-to-encourage-useful-issues-and-pull-requests/creating-a-pull-request-template-for-your-repository)

このブランチでは、Issue / PR テンプレートの作成・編集を行います。今回は行いませんでしたが、GitHub Actions の設定にも。

テンプレートファイルは、`main` ブランチ (厳密にはデフォルト設定のブランチ) から読み込まれます。しかし、`main` に直コミットは避けたい、でも `feature` などを経由せず早く適用したい･･･。

というわけで、これらのファイルをすぐに適用するためのブランチがこれです。

## 運用した所感

### `feature` (+2) の効能

`feature` に切って実装を進めた点については、作業途中のコードがごちゃごちゃになるのを防ぐ効果があったように感じます。

`feature` `fix` `refact` の 3つに分けたので、作業のカテゴリをブランチ名からすぐ判断できて良いかも。

### PR から、こなした作業概要を把握できる

最終的には、コミット数は 350 超え、PR の数は 80 超えとなりました。

350 あるコミットメッセージを眺めるのはちょっとキツいですが、PR の粒度なら作業履歴を追いやすく感じます。なにせ PR はまとまった作業単位で、概要と共に保管されているので。これは複数人開発だとめっちゃ大事になるんじゃないかと思いました。

### `develop` の存在、要らなかったかも

`feature` 上で一通り作業を終えてからマージするので、`develop` も常に動作可能な状態でした。そう考えると、`develop` ブランチは無くても良かったかもと思っています。

リリースの区切りについては、どのみちタグ付け & GitHub でリリース発行するため、わざわざ「`main` へのマージでリリース」とこだわる必要も無かったかも。

## ブランチモデル、調べてみたら･･･

「Git Flow や GitHub Flow、しっかり手順通りじゃ上手くいかなかったからアレンジした」みたいな記事をけっこう見かけます。結局、「プロジェクトの形式は様々だから、共通の最適解なんてないよ！」ってことですね。

ブランチモデルは他の何かをアレンジしたり、あるいは完全独自で作ったりして、自チームに合ったのを模索するのが正解のようです。(当たり前っちゃ当たり前、でもそれしかない！)

## あとがき

一応言っておくとこのブランチモデル、1人開発で 1回しか経験が無いので、複数人になったらどうなるか、他の開発でどうなるか、全然分かりません。

ブランチモデルの定石に近いものはあるかもだが、チームやプロジェクトに応じてアレンジが必要になるのではないか？そんなことを思った、今回の開発体験と記事書きでした。
