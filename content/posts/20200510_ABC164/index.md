---
title: "ABC164"
date: 2020-05-10T01:17:31+09:00
showLastmod: true
draft: false
toc: false
math: true
images:
---

## D問題
https://atcoder.jp/contests/abc164/tasks/abc164_d

### 問題概要
数字で構成された文字列 \(S\) の連続部分文字列のうち，10進数の数値と見なしたとき2019の倍数となるものの個数を出力せよ．

### 制約
- \(1 \leq |S| \leq 2 \times 10^5\)

### 解答
ABC158_Eとほぼ同じ．

\(S\) の下位桁のほうから順に\(\mod 2019\) を見ていって記録する．

条件は「\(\mod 2019\) の値が等しいものを2つ取り出す」と言い換えられるから，
\(\mod 2019\) の値が \(i\)（\(0 \leq i \leq 2018\)）であるものの個数を \(n_i\) としたとき，\(\sum_i {}_{n_i}\mathrm{C}_2\) が解となる．

https://atcoder.jp/contests/abc164/submissions/12381028
