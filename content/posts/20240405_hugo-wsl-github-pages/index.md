---
title: "Hugo+WSL+GitHub Pagesでつくったページに引っ越した時の個人的メモ"
date: 2024-04-05T22:47:50+09:00
showLastmod: true
draft: false
toc: false
mathjax: false
images:
---

4年ほど前にはてなブログで作ってほったらかしてた記事たちを引っ越した．
理由としては，
- 単純にHugoを使ってみたかった
- はてなブログで数式使うのが面倒だった
	- インライン数式が`[tex: hogehoge ]`のような謎記法．誰が得しているんでしょうか
- gitでポストを管理->そのままGitHub Pagesで公開，という簡潔なフローで済む

といった感じ．

環境構築からページ公開までかなり楽に進められたと思うが，何度かつまづくこともあったのでメモしておく．

{{< horizontal_separator >}}

## 環境
- WSL2 Ubuntu 22.04.3 LTS
- Hugo v0.124.1+extended


## はじめに
Hugo+GitHub Pagesの解説をしているサイトは適当に調べるだけでも沢山見つかる．
しかし，Hugo周りの開発のアクティブさから，「言っていることがサイトによって違うんだが？」ということがよく起こる．
それぞれスムーズに進まなかったポイントを抽出して記す．

参考にしたサイト：
- https://gohugo.io/getting-started/quick-start/（公式）
- https://sat8bit.github.io/posts/hugo-with-github-pages/
- https://koyomiji.com/log/72
- https://zenn.dev/okaponta/articles/c302f58507febc

## Hugoの導入
[Hugo公式のインストールガイド](https://gohugo.io/installation/linux/)を読むと，
Ubuntuの場合は`sudo apt install hugo`をしろと書いてある．

が，aptにあるhugoはかなり前のバージョンしかない（2024年4月現在）ため，
[Hugoのリリースページ](https://github.com/gohugoio/hugo/releases)から直接最新のパッケージを持ってくるべき．

`hugo_extended_X.XXX.X_linux-amd64.deb`をダウンロードし，
```bash
sudo dpkg -i hugo_extended_X.XXX.X_linux-amd64.deb
```
でインストールする．

## サイトの設定
基本的に各サイトを参考にしてよさそう．
- テーマの設定
	- [Hugo Themes](https://themes.gohugo.io/)から良い感じのテーマを探して`git submodule add`する
	- `hugo.toml`でテーマを設定
- 適当なページを作ってみる
	- `hugo new content/posts/test.md` のようにする
- ページデザインの変更
	- `theme/theme-name/`下にあるファイルのうち，変更したいファイルを見つける
	- `archetypes/`, `assets/`, `layouts/`など同階層の位置にコピペして，その中で好きなように手を加える

`hugo server`でローカルサーバを立ち上げて見た目を確認することができる．
サーバの再起動なし，かつ爆速で変更が反映される．（すごすぎ！）

## git関連
以下の設定をしてGitHubにpushする．

- `.gitignore`
	- 有志が用意してくれたものを拝借：[Hugo.gitignore](https://github.com/github/gitignore/blob/main/community/Golang/Hugo.gitignore)
- workflow
	- 有志が用意してくれたものを拝借2：[hugo.yml](https://github.com/actions/starter-workflows/blob/main/pages/hugo.yml)
	- これを`.github/workflows/pages.yml`として保存
	- 環境変数を設定してタイムゾーンを`Asia/Tokyo`にしておくことを推奨
		- 参考： https://de-milestones.com/github-action-current-date/
- GitHub Pagesの設定
	- リポジトリ内で Settings > Pages > Source を "GitHub Actions" に指定

サイトによっては，`hugo.toml`に`publishDir = "docs"`を入れないとページが正常に作られない旨の記述があるが，
GitHub Actionsを用いてデプロイする場合は問題なさそうなので必要ない．

{{< horizontal_separator >}}

非常に手軽にデプロイまで済ませることができた．
さらばはてブロくん:grin:
