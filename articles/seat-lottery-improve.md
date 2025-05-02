---
title: "昔作った席替えプログラムの改善点を蔵出し & 改良"
emoji: "🚧"
type: "idea"
topics:
  - "Web"
  - "工業デザイン"
published: true
published_at: "2025-04-30 23:40"
---

## 本記事作成のきっかけ

Note にて、このような記事を書いていたとき。

@[card](https://note.com/akuad/n/nbc8ac16a9175)

作った席替えプログラムを一度作り直したわけですが、作り直し前の改善点を挙げていたところ、そこそこの量になって、それを開発アイデアとして Zenn に投稿したいな、となったわけです。

ということで、本 Zenn 記事タイトルの通り。

(作り直したきっかけは、上の Note を参照してください。)

## 作ったもの - 席替えプログラム

こちらが作り直す前の版。

@[card](https://github.com/aKuad/SeatLottery_old)

こちらが作り直した後の版。

@[card](https://github.com/aKuad/SeatLottery)

## 本題 - 反省点いろいろ

操作で目に入る順に見ていきましょう。ソースコードを蔵出しして撮ったスクリーンショットも一緒に。

### 濃い色の使い過ぎ

![旧版 UI 1](/images/seat-lottery-improve/old-ui-1.webp)

赤、緑、青、黒、灰、そして背景の白･･･。工業デザイン的には、使う色は背景色 + 文字色に + アクセントカラー で 3色、最大でも 4色程度までが限度です。

3色の例: YouTube (アクセントに 赤), Qiita (アクセントに 緑)

4色の例: TikTok (アクセントに 赤 & 青)

あと、数字入力やボタン、BlockA などの表示で、枠線がやたらたくさん目に入るのも気になるところ。

### 完全一方通行 UI

まあまあ、UI の見栄えくらいは我慢できるとして･･･使い心地、こっちのが致命的。先ほどのスクリーンショット右上の 'Next' で次の画面に移ります。

![旧版 UI 2](/images/seat-lottery-improve/old-ui-2.webp)

この画面、よく見てみると･･･？そうです、戻るボタンがありません。座席レイアウト変更に戻れないのです。メンバー (生徒) の入力を進めているうちに「あ、1席足りなかった」なんてなったら、リロードしてやり直しです。

失敗でリセットって、RTA かな？

メンバー入力を終えて次の画面、抽選結果も同じく戻るボタンなし。メンバーのタイポなんかがあれば、これもリセットポイントです。

![旧版 UI 3](/images/seat-lottery-improve/old-ui-3.webp)

### 最終的な印刷イメージが見えないまま進む

Markdown をプレビューみながら編集するように、最終的な完成形を見ながら編集できた方が、普通嬉しいですよね？

ところがこれ、最後の最後まで完成形が見えません。それも一方通行 UI なので、完成形が見えたところで編集に戻れない。

特に、この「ドキュメント情報編集」は、所見では何のこっちゃ？という感じです。タイトルはまだしも、説明(左・右)･･･？

![旧版 UI - ドキュメント情報編集](/images/seat-lottery-improve/old-ui-2-doc-note.webp)

### `innerHTML` へ直書きコード

視点を変えて、内部の話も少し。こちら、一部ソースコード抜粋。

```js
        var changeHTML =
        '<tbody>' +
            '<tr>' +
                '<th class="memberList-cell memberList-cell-block' + blockNameConvert[updateBlock] + '"><input type="checkbox" name="memberCheck' + blockNameConvert[updateBlock] + '" onchange="memberCheckChange(' + updateBlock + ', 0)"></td>' +
                '<th class="memberList-cell memberList-cell-block' + blockNameConvert[updateBlock] + '">出席番号</td>' +
                '<th class="memberList-cell memberList-cell-block' + blockNameConvert[updateBlock] + '" style="width: 200px;">名前</td>' +
                '<th class="memberList-cell memberList-cell-block' + blockNameConvert[updateBlock] + '" style="width: 200px;">読み</td>' +
                '<th class="memberList-cell memberList-cell-block' + blockNameConvert[updateBlock] + '">性別</td>' +
            '</tr>';
```

なんともひどいコードです。当時の僕はまだ、`document.createElement()` というメソッドなどは知らず、`innerHTML` 直の書き込みでしか、HTML のビューを制御する方法を知らなかったのです。

インジェクションの恐れがあったりするので、まともなプロダクトでコレやると、きっとどこかの席から自分の顔面めがけて拳が飛んでくることでしょう。

### 他

- メンバーの「ファイルから追加」で、どういうファイルを入れれば良いかノーヒント
- 重要な操作である抽選ボタンが目立ってなさすぎ
- 「次は抽選ボタンを押す」などの操作説明が一切無し

などなど･･･

![旧版 UI - メンバーを追加](/images/seat-lottery-improve/old-ui-2-member-add.webp)

まぁ、こんな具合ではとても人様には渡せない仕上がりだったわけです。

## 改善後

こちら、作り直した版の UI。

![新版 UI 1](/images/seat-lottery-improve/new-ui-1.webp)

![新版 UI 2](/images/seat-lottery-improve/new-ui-2.webp)

なんということでしょう。あの古いインターネット臭漂う UI から、一気に洗練された UI へと変身したではありませんか。

以下、意識したことなど:

- 色を黒、白、そこにアクセントに青と緑で、色を減らした (あとアクセントは薄い色にした)
- 表を最低限の縦線のみ、数値・テキストの入力欄を下線のみ、座席枠を右と下線のみと、線の数を減らした
- ボタンをアクセントカラー & 丸い角で、ボタンとして目立つ見た目にした
- 騒がしかったメンバー入力 UI は、ファイル読み込みなどなどの機能はばっさりカット
  - ごちゃごちゃやるより、コンマ区切り文字列入力とシンプルにすれば、初見でも迷わないし、機能として十分と判断
- タイトルやメモ欄は、完成形画面に入力できる形にした
- 次に操作すべきことの説明を加えた
- ちゃんと前の画面に戻れるようにした
- もちろん、`innerHTML` 直書きも卒業

## 感想

前作ったものって、時間経って改めて見たらひどい出来だった、ということって多々ありますよね。

でも、下手クソなコードや UI を量産した昔の自分から、もっと良い設計ができる自分につながっていると思えば、頑張った昔の自分を評価してあげたいなって思います。
