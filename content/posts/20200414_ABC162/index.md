---
title: "ABC162"
date: 2020-04-14T14:39:43+09:00
showLastmod: true
draft: false
toc: false
math: true
images:
---

E解きたかったね～～～～

## C問題
### 問題概要
\(\displaystyle\sum_{a=1}^{K} \displaystyle\sum_{b=1}^{K} \displaystyle\sum_{c=1}^{K} \gcd(a,b,c)\) を出力せよ．

### 制約
- \(1 \leq K \leq 200\)

### 解答
愚直にやるとTLEになるかと思ってちょっと手を加えた．（本当は愚直で通るらしい）

\((a,b,c)\) のうちすべて同じ値のときは1通り，2つ同じ値があるときは3通り，すべて違う値のときは6通りの並び方があるので
それぞれのGCDを1倍，3倍，6倍にして加算していく．

https://atcoder.jp/contests/abc162/tasks/abc162_c

https://atcoder.jp/contests/abc162/submissions/11814130
（実装が汚い）


## D問題
### 問題概要
`'R'`,`'G'`,`'B'`からなる長さ \(N\) の文字列 \(S=\{S_1,S_2,...,S_N\}\) について，
以下の2つの条件を満たす \((i,j,k)\) （\(1 \leq i \lt j \lt k \leq N\)）がいくつあるか求める．

- \(S_i \neq S_j\) かつ \(S_j \neq S_k\) かつ \(S_i \neq S_k\)
- \(j-i \neq k-j\)

### 制約
- \(1 \leq N \leq 4000\)

### 解答
まず1つ目の条件のみを考える．

\(k\) を固定したとき，条件を満たす組の数は，\(k\) より前に出てきた \(S\_k\) 以外の2つの文字の組み合わせの数に等しい．
\(1 \leq k \leq N\) について，\(k\) を更新する毎に`'R'`,`'G'`,`'B'`の総数を更新すれば，\(k\) より前に出てきた文字の総数がわかるから，
組み合わせの数を加算していけばよい．

```cpp
int R=0,G=0,B=0;
ll result=0LL;
for(int i=0; i<N; i++) {
	if(S[i]=='R') {
		R++;
		result+=G*B;
	}
	else if(S[i]=='G') {
		G++;
		result+=R*B;
	}
	else if(S[i]=='B') {
		B++;
		result+=R*G;
	}
}
```

ここから2つ目の条件を満たさない組を引けばいい．

https://atcoder.jp/contests/abc162/tasks/abc162_d

https://atcoder.jp/contests/abc162/submissions/11819784


## E問題
### 問題概要
長さ \(N\) の数列 \(\{A_1,A_2,...,A_N\}\) で，\(1 \leq A_i \leq K\)（\(1 \leq i \leq N\)）を満たすすべての数列について，
\(\gcd(A_1,A_2,...,A_N)\) の総和を求める．

### 制約
- \(2 \leq N \leq 10^5\)
- \(1 \leq K \leq 10^5\)

### 解答
\(M\) を公約数に含む数列は，すべての要素が \(M\) を約数に含むものに限られる．

最大公約数が \(M\) である数列は，上記の数列から最大公約数が \(2M, 3M, ...\) である数列を除いたものであるから，
\(M\) を \(K\) から \(1\) まで順に求めることができる．

「\(M\) を約数に含む数」は「\(M\) の倍数」と同値であるから，該当する値は \(\left\lfloor \dfrac{N}{M} \right\rfloor\) だけある．
よって \(M\) を公約数に含む数列は \(\left\lfloor \dfrac{N}{M} \right\rfloor^N\) だけあるが，
愚直にforをぶん回すと（多分）TLEするので繰り返し二乗法でやった．

ビットシフトを初めてまともに使ったかもしれない．bit全探索も覚えたい．

https://atcoder.jp/contests/abc162/tasks/abc162_e

https://atcoder.jp/contests/abc162/submissions/11902215

{{<horizontal_separator>}}

10000人参加おめでとう！

サーバーパンクしてそうで怖い！（最初の20分くらいジャッジ詰まってた気がする）
