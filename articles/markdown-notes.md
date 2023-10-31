---
title: "知らなかった Markdown のあれこれ"
emoji: "📝"
type: "idea"
topics:
  - "markdown"
published: true
published_at: "2023-10-28 15:06"
---

## この記事は？

タイトル通り、Markdown に関して僕が「あ、そうだったんだ」「こういうのあったんだ」と思ったことを、トリビアの泉形式でただただ綴っていきます。

文法の詳しい解説とかは省略しています。各公式ページ見た方が分かりやすいと思いますし。(え？いや、サボりとかじゃないですよそんな･･･)

新たな発見があったら、追加したりしなかったりするかもしれません。

## Markdown は個人開発から始まっている

オリジナルは、John Gruber (ジョン・グルーバー) という方のプロジェクト。

@[card](https://daringfireball.net/projects/markdown/basics)

指定の (しかし単純な) 文法で書かれたテキストを HTML に変換してくれる Perl スクリプトを開発されました。この Perl スクリプトおよびそのプロジェクトが Markdown、そしてその文法が皆さんご存知 Markdown 文法のオリジナルなのです。

狭義の Markdown は、この個人開発によって作られた Perl スクリプトのことを指すと言えそうです。

:::message
**補足トリビア**

このオリジナルの Markdown の文法は、**規定がとても曖昧です**。それ故、各所で Markdown パーサが実装されるにあたり、細かな動作の違いが多々発生することになりました。(そもそもこんな広く使われるのは想定していなかったんでしょう･･･。)

そんな状況を改善すべく、John Gruber とは別の方により、CommonMark という厳密な Markdown 規格が作られました。

@[card](https://commonmark.org/)
:::

## オリジナル Markdown にテーブル (表) と打ち消し線の文法は無い

さて、上の Web ページを読んだ方は気付いたかもしれません。「あれ、テーブルの文法は？」「打ち消し線は？」

```markdown
| Column 1 | Column 2 |
| --- | --- |
| Content 1-1 | Content 2-1 |
| Content 1-2 | Content 2-2 |
```

```markdown
~~Strikethrough~~
```

ここで GitHub Flavored Markdown の文法を見てみましょう。

@[card](https://github.github.com/gfm/)

Tables と Strikethrough に (extension) の表示が。

そう、テーブルの規定は拡張仕様なんです。多くのサービスが GitHub Flavored Markdown ベースの Markdown を実装しているようで、テーブルや打ち消し線が使えるところは多いですね。

おかげでてっきり標準規定かと･･･。

## Zenn/Qiita/GitHub には補足を示せる文法がある

```markdown
:::message
ここで言う補足というのは、このようなテキスト枠のこと。
:::
```

:::message
ここで言う補足というのは、このようなテキスト枠のこと。
:::

Qiita では `:::note` が同等の機能。

一度や二度は見かけたことはあったはずが、僕はそれをすっかり忘れて代わりに引用表記をよく使っていました (at Qiita)。

> ちなみに引用表記はコレのこと

@[card](https://zenn.dev/zenn/articles/markdown-guide#%E3%83%A1%E3%83%83%E3%82%BB%E3%83%BC%E3%82%B8)

@[card](https://qiita.com/Qiita/items/c686397e4a0f4f11683d#note---%E8%A3%9C%E8%B6%B3%E8%AA%AC%E6%98%8E)

割と最近 (2022/05/19)、GitHub にも似たようなものが追加されました (公式 Docs では警告と呼ばれている)。

```markdown
> [!NOTE]
> Any note here
```

@[card](https://github.com/orgs/community/discussions/16925)

@[card](https://docs.github.com/ja/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax#alerts)

:::message
**補足トリビア**

VSCode 拡張の Markdown Preview Enhanced にも同様の機能があります。こちらはより充実していて、Note だけでなく Question とか Bug とか色々あります。

@[card](https://squidfunk.github.io/mkdocs-material/reference/admonitions/#supported-types)

Material for MkDocs という Markdown 静的サイトジェネレータの機能を利用しているようです。
:::

## Zenn/Qiita にはコード差分を示せる文法がある

````markdown
```diff c
+   printf("Hello\n");
-   printf("World\n");
    return 0;
```
````

```diff c
+   printf("Hello\n");
-   printf("World\n");
    return 0;
```

これを知らずに、よくコメントアウトで `// 追加` とか書いてました。(でも場合によってはコメントアウトの方が便利かも？コピペ前提で貼るときとか。)

@[card](https://zenn.dev/zenn/articles/markdown-guide#diff-%E3%81%AE%E3%82%B7%E3%83%B3%E3%82%BF%E3%83%83%E3%82%AF%E3%82%B9%E3%83%8F%E3%82%A4%E3%83%A9%E3%82%A4%E3%83%88)

@[card](https://qiita.com/Qiita/items/c686397e4a0f4f11683d#diff%E3%81%A8%E4%BB%96%E3%81%AE%E8%A8%80%E8%AA%9E%E3%81%AE%E3%82%B7%E3%83%B3%E3%82%BF%E3%83%83%E3%82%AF%E3%82%B9%E3%82%92%E5%90%8C%E6%99%82%E3%81%AB%E4%BD%BF%E3%81%86)

## 第1レベル見出しは文書先頭 1度のみの使用が推奨されている

Markdown Lint に規定が、それから Zenn の Markdown チートシートにも推奨事項として記載されています。

@[card](https://github.com/DavidAnson/markdownlint/blob/v0.31.1/doc/md025.md)

@[card](https://zenn.dev/zenn/articles/markdown-guide#%E8%A6%8B%E5%87%BA%E3%81%97)

それから、Qiita の Markdown チートシートのページの Markdown ソースも見てみると、ここも第 2レベル以下の見出しが使われているのが分かります。

@[card](https://qiita.com/Qiita/items/c686397e4a0f4f11683d.md)

## あとがき

たまに仕様書とか公式リファレンスとか読んでみると、「あ、そんな機能あったんだ」なんていう発見が割とあります。ここでは Markdown チートシートがそれにあたります。

知ってる言語やソフトも、たまには再入門してみると良いかもしれません。
