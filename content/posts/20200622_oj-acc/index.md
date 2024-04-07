---
title: "online-judge-tools と atcoder-cli の環境構築したときのメモ的なアレ"
date: 2020-06-22T00:09:24+09:00
showLastmod: true
draft: false
toc: false
math: false
images:
---

難しいことや詳しいことはともかく，とりあえず動かしたいひと向け

公式等で説明があるところは省いてるところも多いのでそっちも見てください

{{<horizontal_separator>}}

## online-judge-tools, atcoder-cli って何
テストケースの正誤確認，コードの提出をコマンドラインからやってくれる便利なツール．
公式のチュートリアルや解説サイトも調べればあるので詳しいことは省略．

[Getting Started for oj command (日本語)](https://github.com/online-judge-tools/oj/blob/master/docs/getting-started.ja.md)

[コマンドラインツールatcoder-cliを公開しました](http://tatamo.81.la/blog/2018/12/07/atcoder-cli/)


## インストール
こちらを参考にしてインストールする．（atcoder-cli開発者の方の記事）

[atcoder-cli インストールガイド](http://tatamo.81.la/blog/2018/12/07/atcoder-cli-installation-guide/)


## できること
### コンテスト別のディレクトリ作成
カレントディレクトリに指定コンテストのディレクトリを作成する．
```bash
acc new contestID
```
`contestID`はAtCoderのコンテストページのURLを見るとわかる．

#### ex) AtCoder Beginner Contest 170の場合
URL: https://atcoder.jp/contests/abc170 -> "abc170"が`contestID`

#### ex) 東京海上日動 プログラミングコンテスト2020の場合
URL: https://atcoder.jp/contests/tokiomarine2020 -> "tokiomarine2020"が`contestID`



ここでテストケースをダウンロードする問題を選択する．
後で別の問題のテストケースをダウンロードするときは`acc add`コマンドを使う．

ダウンロードに成功すると，コンテストのディレクトリ（/abc170など）の直下に問題別ディレクトリ（/abc170/aとか）が作成され，その中にテストケースのファイルが入ったディレクトリ（/abc170/a/tests）が作成される．

```
abc170
┠ a
┃┗ tests
┃　┠ sample1.in
┃　┠ sample1.out
┃　┠ sample2.in
┃　┗ sample2.out
┠ b
┃┗ tests
┃　┠ sample1.in
┃　┠ sample1.out
┃　┠ sample2.in
┃　┠ sample2.out
┃　┠ sample3.in
┃　┗ sample3.out
┃
（つづく）
```

テストケース確認やコード提出は問題別ディレクトリ内で行う．

### テストケースの確認
問題別ディレクトリにソースコード（ここでは`main.cpp`）を置くことで，テストケースのコピペをせずに出力の確認を行うことができる．
```bash
oj t -d ./tests
```

`-d ./tests`はテストケースが入ったディレクトリの指定．

online-judge-toolsがテストする実行ファイルはデフォルトで`a.out`と決められているので，MinGWのg++でコンパイルするときは
```bash
g++ main.cpp -o a.out
```

と出力先の名前を指定する．


### コードの提出
```bash
acc s main.cpp
```
で`main.cpp`の内容を提出できる．
実行ディレクトリから勝手にどの問題に対する提出か判定してくれる．

このとき，
```
the problem "(problem URL)" is specified to submit, but no samples were downloaded in this directory. this may be mis-operation
```

とエラーが出ることがあるが，これはテストケースをダウンロードするとき，atcoder-cliからした場合とonline-judge-toolsからした場合でディレクトリ構造が異なることに起因するらしい？

参考：
- https://github.com/Tatamo/atcoder-cli/pull/9
- https://github.com/online-judge-tools/oj/issues/336

一度解決したかと思ったけどやっぱり再発するので，だれか解決策教えてください．

### テンプレートの作成
例としてC++のテンプレートをつくる．

以下のコマンドでatcoder-cliのconfigディレクトリをみつける．
```bash
acc config-dir
```

configディレクトリ内にcppディレクトリを作成する．
ここで作成したディレクトリの名前がそのままテンプレート名になる．

cppディレクトリの中に`template.json`を作成する．内容は以下のリンクを参照．
https://github.com/Tatamo/atcoder-cli#readme

とりあえず設定すべきところを挙げる．
#### .task.program
問題別ディレクトリを作成するときに，指定したソースファイルのコピーを置いてくれる．超便利．
```json
"program": [["A.cpp","B.cpp"]]
```
このように書くと，config\cppディレクトリ内にある A.cpp のコピーを "B.cpp"というファイル名で各問題別ディレクトリに置いてくれる．

```json
"program": ["main.cpp"]
```
このように書くと，config\cppディレクトリ内にある main.cpp のコピーを "main.cpp"というファイル名で各問題別ディレクトリに置いてくれる．

```json
"program": [["C:\Users\username\Desktop\temp.cpp","main.cpp"]]
```
フルパスでの参照も可能．
デスクトップにある temp.cpp のコピーを "main.cpp"というファイル名で各問題別ディレクトリに置いてくれる．

```json
"program": [["temp1.cpp","main.cpp"],["temp2.cpp","sub.cpp"]]
```
複数のソースファイルをコピーすることもできる．

#### .task.submit
あらかじめ提出するソースファイル名を決めておくことで，提出コマンドが
```bash
acc s
```
だけで済む．

.task.program で決めたコピーファイル名と同じものにすることを推奨．
```json
"submit": ["main.cpp"]
```
提出コマンドを打つと main.cpp を提出してくれる．

### テンプレートの適用
コマンドにより作成される問題別ディレクトリに適用するテンプレートを指定する．
```bash
acc new|add --template template_name
```


## やっといた方がいいと思う設定

### テストケースのディレクトリ名
テストケースを保存するディレクトリのデフォルト名を "test" とすることで，テストケース確認時のコマンドが
```bash
oj t
```
のみで済む．

以下のコマンドで設定．
```bash
acc config default-test-dirname-format test
```

### デフォルトテンプレ―ト
普段よく使うテンプレートがあるならデフォルトにするとよい．

この設定をすることで，`acc new|add`コマンドで `--template`オプションをつけなくてもよくなる．
```bash
acc config default-template template_name
```

{{<horizontal_separator>}}

何か思い出したり，設定をいじったら追記します
