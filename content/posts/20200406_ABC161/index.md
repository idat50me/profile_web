---
title: "ABC161"
date: 2020-04-06T22:23:09+09:00
showLastmod: true
draft: false
toc: false
math: true
images:
---

3完ﾋｴﾋｴ

## C問題
### 問題概要
整数 \(N\) に対し以下の操作を \(0\) 回以上行ったときの \(N\) の最小値を出力する．

操作：\(N=|K-N|\)

### 制約
- \(0 \leq N \leq 10^{18}\)
- \(1 \leq K \leq 10^{18}\)

### 解答
\(N \gt K\) のとき，操作は \(N=N-K\) であり，\(N \leq K\) になるまでこれを繰り返せる．
この操作は \(\frac{N}{K}\) 回行うことができる．

\(N \leq K\) のとき，\(N\) の遷移は \(N,K-N,N,K-N,...\) となるから，\(\min(N,K-N)\) が答えである．

https://atcoder.jp/contests/abc161/tasks/abc161_c

https://atcoder.jp/contests/abc161/submissions/11510455
