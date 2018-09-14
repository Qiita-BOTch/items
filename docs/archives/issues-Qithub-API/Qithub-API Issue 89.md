これは  リポジトリの[ issue をアーカイブ]()したものです。

# `roll-dice` プラグインの新準拠と info.json 追加

- 2018/01/18 11:53 by KEINOS
- State: closed
- Archive of https://api.github.com/repos/Qithub-BOT/Qithub-API

## 本文

Issue #77 の`system/` と `plugins/` の統合により以下を実施。

- `info.json` ファイルの設置（ヘルプ・バージョン対応）
- REST 経由とトゥート経由でプラグインの実行コマンドを共通にした。（`args`で渡す）

## プラグインのコマンドの渡し方
- 従来の渡し方
    - `&dice_code=‘3d5’&use_emoji=‘yes’`
- 新仕様の渡し方
    - `&args='3d5 —use_emoji'`

## 動作テスト

クエリの `&mode=debug` を削除すると Qithub エンコードデータで取得できます。

- `@qithub:roll-dice —version`<br>https://blog.keinos.com/qithub_dev/?plugin_name=roll-dice&args=--version&mode=debug
- `@qithub:roll-dice —help` <br>https://blog.keinos.com/qithub_dev/?plugin_name=roll-dice&args=--help&mode=debug
- `@qithub:roll-dice 3d5`<br>https://blog.keinos.com/qithub_dev/?plugin_name=roll-dice&args=3d5&mode=debug
- `@qithub:roll-dice 3d5 --use_emoji`<br>https://blog.keinos.com/qithub_dev/?plugin_name=roll-dice&args=3d5%20--use_emoji&mode=debug
- `@qithub:roll-dice`<br>https://blog.keinos.com/qithub_dev/?plugin_name=roll-dice&args=&mode=debug

-----

## コメント

-----

##### 358639387

2018/01/18 12:59 by hidao80

LGTM👍

roll-dice がいい叩き台になってよかった！ トゥートとの連携が見えてきた気がします！😁

-----

##### 358649612

2018/01/18 13:42 by KEINOS

> roll-dice がいい叩き台になってよかった！ 

まさに！他のプラグインと旧`system`のスクリプトが`plugins`に統合されたらサーバーの引っ越しです！

デプロイしマスター！以下、デプロイ先のテストです。問題なければ、お手すきにブランチ削除お願いし 💪 

## 動作テスト

クエリの `&mode=debug` を削除すると Qithub エンコードデータで取得できます。

- `@qithub:roll-dice —version`<br>https://blog.keinos.com/qithub/?plugin_name=roll-dice&args=--version&mode=debug
- `@qithub:roll-dice —help` <br>https://blog.keinos.com/qithub/?plugin_name=roll-dice&args=--help&mode=debug
- `@qithub:roll-dice 3d5`<br>https://blog.keinos.com/qithub/?plugin_name=roll-dice&args=3d5&mode=debug
- `@qithub:roll-dice 3d5 --use_emoji`<br>https://blog.keinos.com/qithub/?plugin_name=roll-dice&args=3d5%20--use_emoji&mode=debug
- `@qithub:roll-dice`<br>https://blog.keinos.com/qithub/?plugin_name=roll-dice&args=&mode=debug

-----

##### 358666356

2018/01/18 14:41 by hidao80

LGTM👍

P.S.
> 問題なければ、お手すきにブランチ削除お願いし 💪

なんですけど、なんだか年明けくらいから**マージしたブランチが自動消滅している**ようなのです。

マージするときに「マージして新しい master を作るぜ？」(意訳) みたいな選択肢が出るようになっているので、そのせいかもしれません。要調査です。

-----

##### 358773854

2018/01/18 20:36 by KEINOS

マージありがとうござい 💪 !

### 消えるけど消えないブランチ問題

>> 問題なければ、お手すきにブランチ削除お願いし 💪
>
> なんですけど、なんだか年明けくらいから**マージしたブランチが自動消滅している**ようなのです。

アチキの画面では、下記図のようにブランチが残っているのですが、原因がわかりました！

<kbd><img width="100%" alt="2018-01-19 4 50 15" src="https://user-images.githubusercontent.com/11840938/35118590-8c7a772a-fcd5-11e7-923c-b1f51f032a71.png"></kbd>

アチキの[フォークした REPO](https://github.com/KEINOS/Fork_Qithub-BOT_scripts) で作成した作業ブランチからの PR の仕方が原因のようで、以下の２つが考えられます。

1. 自身の `master` にマージせずに本家の `master` に 直接 PR を出している
2. 本家には該当ブランチがないから（本家にブランチを `Push` していないから）

(1) ですが、本家の `master` にマージされたあと（他者のコードレビューを終えたあと）に、本家の `master` を アチキのフォークの `master` にアップデートするようにして、常に本家に追随するようにしているからです。

これは PR 時にアチキの `master` ブランチ でなく作業ブランチを PR した方がわかりやすい（マージのミスが少ない）というのもあります。

そして、おそらく **(2) が自然消滅している原因** で、上記図にある「Delete branch」ボタンは (1) により残った私のフォーク先のリポジトリのブランチの削除を促しているのだと思います。つまり、**本家にはそもそもブランチがないので自然消滅したように見える**のだと思います。

以前までは、タスクとして本家にブランチがあったので消えるように見えたのだと思います。

次回 PR で動作を少し注意してみますね。新しいことがいっぱい。



-----

##### 358791985

2018/01/18 21:45 by hidao80

なるほど！ つまり、

1. fork した REPO 内の ブランチを Organization (以下 Org) の REPO に Push しようとしている
1. Org の REPO 内には**そもそもマージしようとしているブランチがない**（上記理由による）
1. Org の REPO からブランチの削除ができない

という流れですね。

そもそも、Org にマージしたブランチをたちどころに削除するのは、**REPO 内にブランチが増殖して収集つかなくなってしまうのを防ぐため**なので、**@KEINOS さん REPO にブランチが残っていても Org 的には構わない**です。😜

### 提案（案A）

**fork した REPO からの PR に使われたブランチの処遇は Org のブランチ運用方針に従わなくても良い**ことにしませんか？

もとより、人様の REPO のブランチを第三者が削除することができなんですけれども。😅

これで、また一つ謎が解けましたね。う〜ん、Github、奥深し。

-----

##### 359166118

2018/01/20 11:54 by KEINOS

> ### 提案（案A）
> fork した REPO からの PR に使われたブランチの処遇は Org のブランチ運用方針に従わなくても良いことにしませんか？

👍 それが良いと思います。
