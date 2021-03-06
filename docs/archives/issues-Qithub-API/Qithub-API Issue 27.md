これは  リポジトリの[ issue をアーカイブ]()したものです。

# スクリプト内のコメントの記述について if-else

- 2017/10/15 02:14 by KEINOS
- State: closed
- Archive of https://api.github.com/repos/Qithub-BOT/Qithub-API

## 本文

## 相談

ちょっとした相談です。PHP の `IF`文の分岐において処理のコメントの付け方ですが、以下の `else`の前後の２パターンのうち、どちらが読みやすい（自分好み）ですか？

### コメント例１
```
// キャッシュの読み込み
if ($use_cash == YES) {
    //do something
// データの新規取得とキャッシュの保存
} else {
    //do something
}
```
### コメント例２
```
// キャッシュの読み込み
if( $use_cash == YES ) {
    //do something
}
// データの新規取得とキャッシュの保存
else {
    //do something
}
```
## 自分の案

個人的には後者（例２）の方が見やすいのですが、[PSR-2](https://qiita.com/hshimo/items/04be1f432240c58300f4) を適用すると以下のようになるので、**前者（例１）かな**、と思います。

```
// キャッシュの読み込み
if ($use_cash == YES) {
    //do something
} // データの新規取得とキャッシュの保存
else {
    //do something
}
```

## 補足

PHPのコード規約については、開発の流れを見ている最中なのでまだ特別に規定はしていませんが、現状 `CodeSniffer`で「[PSR-2](https://qiita.com/hshimo/items/04be1f432240c58300f4)」をクリアすればOKとしています。（本当はWorpressのコード規約の方が見やすいと思っているのですが、そこは好みなので）

なお、コード規約については 新着Qiita記事 の定例トゥートが実装されてから別 issue でディスカッションしたいと考えています。

### 参考文献
- PHPコーディング規約まとめ - @ Qiita<br>https://qiita.com/hshimo/items/04be1f432240c58300f4
----------------

- [x] 過去の Issues も検索しましたが該当するものはありませんでした。

----------------
## TL;DR（結論 2017/10/15 現在）

コメント行は、スコープ（有効範囲を中心としたブロック）単位で考える。
- スコープ外：当該スコープ全体の機能についての記述
- スコープ内：コメント以下の連続するコードが実現できる挙動

```
// キャッシュ処理
if ($use_cash == YES) {
    // キャッシュの読み込み
    //do something
} else if ($use_case == NO) {
    // データの新規取得とキャッシュの保存
    //do something
} else {
    // イレギュラー処理
    //do something
}
```


-----

## コメント

-----

##### 336682813

2017/10/15 03:03 by hidao80

むむむ。微妙ですね。😅

- コメント例1 だと、**if 文の返り値に対するコメントに見える。**
- コメント例2 だと、**コードブロックが不連続に見える**

PSR-2 はさすがとしか言いようのないコメントの書き方です。  
ですが、2択となれば「コメント例2」を推します。

理由は、**コメント例1ではコメント対象のコードと異なるスコープにコメントがあり、コメントとコードに連続性がない**からです。

-----

##### 336701624

2017/10/15 10:25 by KEINOS

> コメントとコードに連続性がない

おっしゃるように [StackOverflow](https://stackoverflow.com/questions/46752448/best-way-to-comment-on-if-else-statement-with-php-psr-2/46753809#46753809) でも、スコープ外に記載するのはクラスや関数などで各分岐の説明はそのスコープ内に記載した方がよく、PSR-2 そのものはコメントの書き方に関しては制限は設けていないため自由に記載して構わないが、`else` は `} else {` としないといけないのと、`else` 処理を後日移動や削除する場合などを考えてブロック単位で考えた方が良いという意見でした。

つまり以下のような書き方の方が良いという感じですかね？

```
// キャッシュ処理
if ($use_cash == YES) {
    // キャッシュの読み込み
    //do something
} else {
    // データの新規取得とキャッシュの保存
    //do something
}
```


-----

##### 336703510

2017/10/15 10:55 by hidao80

That's right! 👍😆

その書き方に賛成です！

個人的なスタイルでは、

- スコープ外上に「当該スコープで行う機能についての記述」を書く
- スコープ内で連続するコードの上に「ここから一連のコードで実現できる挙動」を書く

かつ

- 開き中括弧 `{` は行末に書く（＝行頭に書かない）
- else は `} else {` または `} else if (...) {` と書く

イメージでしょうか。

-----

##### 336704187

2017/10/15 11:08 by KEINOS

すっきりんこです。 👍 

実は、ずっと `} else {` の上にコメント書くのに違和感があったんですが、今までの環境の癖とは怖いもので、容認してました。 😅 

わかりづらいとか、こういうコメントの方がいい、などあったらバンバン言ってください！
人様がわかってのコメントですから。（あ、未来の自分もか）


-----

##### 336704900

2017/10/15 11:22 by KEINOS

TL;DR を更新しました！

-----

##### 336705639

2017/10/15 11:37 by hidao80

この記事のとおりだと思います。  
ガンガン突っ込みますよ！😄

Qiita記事: コードレビューの極意。それは「自分のことは棚に上げる」こと！！ <https://qiita.com/jnchito/items/0a0b46106681f41f2f0e>

-----

##### 336706240

2017/10/15 11:49 by KEINOS

👍 もぅ社会の窓 全開 で人の身だしなみをしつける勢いで行きましょう！
