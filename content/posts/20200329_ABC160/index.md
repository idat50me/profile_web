---
title: "ABC160"
date: 2020-03-29T22:02:02+09:00
showLastmod: true
draft: false
toc: false
math: true
images:
---

## C問題
### 問題概要
\(1\) 周 \(K\) メートルの円形の湖の周りに \(N\) 軒の家がある．

\(i\) 番目の家は，湖の北端から時計回りに \(A_i\) メートルの位置にある．

いずれかの地点からすべての家を訪ねるための最短距離を出力する．

### 制約
- \(2 \leq K \leq 10^6\)
- \(2 \leq N \leq 2 \times 10^5\)
- \(0 \leq A_1 \lt ... \lt A_N \lt K\)

### 解答
隣同士の各家について間隔の最大値 \( d_{\max}\) をとれば答えは \( K-d_{\max}\) である．
制約より \( A_i\) はソートされた状態で入力されるので，入力毎に各家間の距離を計算し，\( d_{\max}\) を更新すればよい．
最後に \( A_1\), \( A_K\) 間の距離を例外的に計算する．

https://atcoder.jp/contests/abc160/tasks/abc160_c

https://atcoder.jp/contests/abc160/submissions/11282977

## D問題
### 問題概要
頂点 \(N\) 個の無向グラフ \(G\) がある．\(G\) は，

- \( i=1,2,...,N-1\) について頂点 \(i\) と頂点 \(i+1\) 間
- 頂点 \(X\) と頂点 \(Y\) 間

に辺をもつ．

頂点 \(i\), \(j\) (\( 1 \leq i \lt j \leq N\)) 間の距離を \(k\) としたとき，
各 \(k(=1,2,...,N-1)\) を満たす整数組 (\(i, j\)) の個数を出力する．

### 制約
- \( 3 \leq N \leq 2 \times 10^3\)
- \( 1 \leq X,Y \leq N\)
- \( X+1 \lt Y\)

### 解答
すべての頂点についてBFSするだけでも \(O(N^2)\) で間に合うが，気づかず場合分けゴリ押し解で通してしまった．

\( D = Y-X\) とすると，

- \( i \leq X\) かつ \(Y \leq j\)
- \( X \leq i \leq X + \dfrac{D+1}{2} - 1\) かつ \(Y \leq j\)
- \( i \leq X\) かつ \(Y - \dfrac{D+1}{2} + 1 \leq j \leq Y\)
- \( X \leq i \leq X + \dfrac{D+1}{2} - 1\) かつ \(Y - \dfrac{D+1}{2} + 1 \leq j \leq Y\)

で場合分けした．ただし，\(i=X,j=Y\) の場合の重複に気を付ける必要がある．

https://atcoder.jp/contests/abc160/tasks/abc160_d

https://atcoder.jp/contests/abc160/submissions/11313172

## E問題
### 問題概要
\(A\) 個の赤リンゴ（おいしさ\( p_i\)），\(B\) 個の緑リンゴ（おいしさ\( q_i\)），\(C\) 個の無色リンゴ（おいしさ\( r_i\)）がある．
無色リンゴは着色して赤または緑と見なすことができる．

赤を \(X\) 個，緑を \(Y\) 個食べた時のおいしさの総和の最大値を出力する．

### 制約
- \( 1 \leq X \leq A \leq 10^5\)
- \( 1 \leq Y \leq B \leq 10^5\)
- \( 1 \leq C \leq 10^5\)
- \( 1 \leq p_i, q_i, r_i \leq 10^9\)

### 解答
すべてのリンゴをまとめてソートして \(X,Y\) の制限を超えないように上から選択するだけ．

`pair`で色情報を持たせながらソートしているが，最初からおいしさ順に赤を \(X\) 個，緑を \(Y\) 個選択して無色に混ぜれば，
そのまま上から選択するだけで条件を満たすことができる．

https://atcoder.jp/contests/abc160/tasks/abc160_e

https://atcoder.jp/contests/abc160/submissions/11307824

## まとめ
最短路問題はBFS！（かダイクストラ法）

それにしても参加者がめちゃ多い
