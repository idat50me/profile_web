---
title: "Misskey上で競プロのコンテスト情報を確認できるウィジェット"
date: 2024-12-06T00:00:00+09:00
showLastmod: false
draft: false
toc: false
math: false
---

Misskeyのウィジェット欄に直近のコンテスト情報を表示させるAiScript Appを作ったので，使ってみてね．

{{< linkcard "https://misskey.kyoupro.com/play/a0kzsfqycd" >}}

{{< horizontal_separator >}}

## 実際の表示
AtCoder, Codeforces, yukicoderで現在開催中または開催予定のコンテストを一覧表示する．
コンテスト名をクリックすると，各コンテストページに飛ぶ．

長期コンテストのみ，終了までの残り期間を表示する．
{{< figure src="widget.png" width="350px" >}}

## 導入手順（簡単に）
1. [Competitive-Programming Contest List (Misskey Play)](https://misskey.kyoupro.com/play/a0kzsfqycd)のページ下部にある「ソースを表示」からソースコードをコピー
1. ウィジェット欄に「AiScript App」を追加
{{< figure src="widget_list_add.png" width="300px" >}}
1. 追加されたAppの設定を開き，「script」にソースコードをペースト
{{< figure src="widget_app_setting.png" width="300px" >}}
1. （オプション）`Config`の各値を好みに合わせて設定する
{{< figure src="config.png" width="380px" >}}

## 動作の全体像
{{< figure src="misskey_contest_widget_overview.png" width="370px" >}}

### データ取得部
GASスクリプトのソースはこちら↓

{{< linkcard "https://github.com/idat50me/cp-contest-data-to-msky/blob/main/Code.gs" >}}

[CLIST API](https://clist.by/api/v4/doc/)と[yukicoder API](https://petstore.swagger.io/?url=https://yukicoder.me/api/swagger.yaml)を叩いてコンテストデータを取得し，コンテストの名前・URL・開催日時等をまとめたJSONに整形したものをMisskeyのページに上書き更新する．
このスクリプトを2時間おきに自動実行するようトリガーを設定しておく．

元々はウィジェットからCLIST APIのRSSを取得するつもりでいたが，
- APIのauth keyを直接書き込む必要がある
- リロードする度にAPIを叩くことになり微妙
- 現在CLISTがyukicoderのコンテスト情報を取得できていない + yukicoder APIがRSS形式をサポートしていない

等の理由から見送った．

Misskey APIの仕様はここを参照した↓

{{< linkcard "https://misskey.kyoupro.com/api-doc" >}}

### データ表示部
ウィジェットのAiScriptが担う．

`Mk:api("pages/show", options)`でページに保存したJSON文字列を取得し，これを基にコンテストの一覧リストを表示する．
10分ごとに終了したコンテストの除外処理とリストの再作成を行うことで準リアルタイム更新っぽくしている．

AiScriptの記法などは次のリファレンス等を参考にした↓

{{< linkcard "https://misskey-hub.net/ja/docs/for-developers/plugin/plugin-api-reference/" >}}

{{< linkcard "https://github.com/aiscript-dev/aiscript/tree/0.19.0/docs" >}}

最近になって[AiScriptのリファレンスページ](https://aiscript-dev.github.io/ja/references/syntax.html)もリリースされたが，0.19.0以前のバージョンと互換性のない最新バージョンの仕様説明になっている．
そのため，各Misskeyサーバーが対応しているAiScriptバージョンに応じたリファレンスを探す必要がある．

Misskeyのページ機能を経由する構成は[こちらの記事](https://qiita.com/the_quick_fox/items/f765659d00f6eefe8b50)を参考にした．今回のウィジェットに限らず使えそう？

## 問題点
リモートサーバーを含む外部のAPIを叩く`Mk:apiExternal()`が[セキュリティ上の問題から削除された](https://github.com/MisskeyIO/misskey/pull/281)ことにより，競プロ鯖のページの情報を他のMisskeyサーバーで取得することができないため，そのままでは競プロ鯖以外のサーバーで動作しない．

他サーバーで動かしたい人は上記のGASのAPI keyやAiScriptのページ指定等をご自身のものに設定して動かしてみてください．
