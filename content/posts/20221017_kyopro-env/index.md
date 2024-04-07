---
title: "競プロ環境構築（WSL2 + VS Code）"
date: 2022-10-17T02:08:02+09:00
showLastmod: true
draft: false
toc: false
math: false
---

デスクトップPCに競プロ環境を入れた時の備忘録．

- Windows 上に gcc が使える環境を作るのかクソ面倒
- WSL のセッティングがかなり手軽にできるようになった

等の理由から，現状 Windows にプログラミングの環境を作るなら競プロに限らず WSL を利用することを強く推奨する．

## 0. 追記
2023/7/13: [**競プロ用に Ubuntu をコピー**](#競プロ用に-ubuntu-をコピー) の記述を変更

2023/2/13: [**競プロ用の WSL2 環境を作成**](#2-競プロ用の-wsl2-環境を作成) を追記

{{<horizontal_separator>}}


## 1. Windows Terminal と Ubuntu のインストール・設定
基本的にこちらの記事と同じ流れ．
基本 Microsoft Store からインストールして設定するだけなので，とても簡単．

[Linux初心者がWSL2とWindows Terminalのセットアップをやってみた](https://dev.classmethod.jp/articles/linux-beginner-wsl2-windowsterminal-setup/)

異なる箇所は以下の通り．

- Linuxディストリビューションは Ubuntu (22.04.1 LTS ?) にした
- Ubuntu のユーザ設定が一部 GUI になっててたまげた
- Ubuntu のデフォルトユーザが root になっていたので，PowerShell で
```shell
ubuntu config --default-user (username)
```
をした

### Windows Terminal の外観設定
Windows Terminal の`setting.json`で，`profiles.defaults`を以下のように設定．
```
"defaults": 
        {
            "colorScheme": "One Half Dark",
            "cursorShape": "vintage",
            "font": 
            {
                "face": "Migu 1M",
                "size": 12
            },
            "opacity": 70,
            "useAcrylic": true
        }
```

フォント「Migu 1M」はこちらからダウンロード．
https://mix-mplus-ipa.osdn.jp/migu/#migu1m

## 2. 競プロ用の WSL2 環境を作成
### ~~競プロ用に Ubuntu をコピー~~
{{< admonition info "追記" >}}
わざわざ競プロ用にインスタンスを分けるメリットよりデメリットの方が強く感じられるようになってきたので、やめました
{{< /admonition >}}

競プロ用にエイリアスとか設定したいが，他の用途で Ubuntu を使うときにも設定されているとなんか気持ち悪いので，競プロ用に Ubuntu をコピーしておく．
https://zenn.dev/souyakuchan/articles/3fb375c23a850e1a9706

Docker のコンテナみたいな感覚でコピーできるらしい（すごい）

PowerShell で，使用している Ubuntu の名前を確認
```
> wsl -l -v
  NAME      STATE           VERSION
* Ubuntu    Running         2
```

WSL2 のデータを保存しておきたい場所で競プロ用の環境を export, import
```
> wsl --export Ubuntu kyopro.tar
> wsl --import kyopro ../wsl_container/kyopro kyopro.tar
> wsl -l -v
  NAME      STATE           VERSION
* Ubuntu    Running         2
  kyopro    Stopped         2
```

### Windows Terminal で競プロ用の環境で実行するプロファイルを作成
Windows Terminal の設定から空のプロファイルを作成し，以下のように設定する

- 名前：（競プロ用とわかる名前にする）
- コマンドライン：`C:\Windows\system32\wsl.exe -d kyopro`

個人的に競プロ関係のファイルは `D:\kyopro\` 直下にまとめたかったので，

- 開始ディレクトリ：`/mnt/d/kyopro`

も設定

### root でログインしないようにする
import 後の環境はデフォルトで root ログインになっているようなので，一般ユーザログインにする

`\\wsl$\kyopro\etc\wsl.conf` に
```
[user]
default=username
```
を追記する

https://github.com/Microsoft/WSL/issues/3974


## 3. atcoder-cli と online-judge-tools の導入
競プロ用の環境に入り，こちらの記事を参考に進める．
http://tatamo.81.la/blog/2018/12/07/atcoder-cli-installation-guide/

oj に PATH が通っていなかったので，ページ下部のトラブルシューティングを参考に通した．

とりあえず acc と oj のログインまでやる．

## 4. atcoder-cli の設定
### cpp テンプレートの作成
```shell
acc config-dir
```
で acc の config ディレクトリを見つけ，直下に cpp ディレクトリを作成．
さらに

- `cpp/template.json` （内容は以下の通り）
- `cpp/main.cpp`

を作成する．

`cpp/template.json` の内容：
```template.json
{
	"task": {
		"program": ["main.cpp"],
		"submit": "main.cpp"
	}
}
```

作成したテンプレートをデフォルトに設定する．
```
acc config default-template cpp
```

### テストケースのディレクトリ名
```
acc config default-test-dirname-format test
```
`oj t` のテストケース名のデフォルトに合わせるため．

### すべての問題を自動で読み込む
```
acc config default-task-choice all
```
これをしないと，contest ディレクトリ作成時にどの task を読み込むか聞かれる．


## 5. VS Code の設定
WSL2 上で VS Code を使うために以下の拡張機能を入れる．
https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl

競プロディレクトリの `.vscode/c_cpp_properties.json` をいじる．とりあえず以下の変更をしておけばよさげ
```
"configurations": [
        {
            "name": "WSL",
            "compilerPath": "/usr/bin/g++",
            "cppStandard": "c++20"
        }
    ]
```

VS Code の左タブから Remote Explorer を選択し，WSL: kyopro に入って競プロ用のディレクトリを開くと，WSL2 の gcc の path が通った状態になる．
