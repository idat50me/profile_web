---
title: "ABC163"
date: 2020-04-30T17:48:33+09:00
showLastmod: true
draft: false
toc: false
math: true
images:
---

unratedです（5/3のABC166が補填コンテストということらしい，ありがたい話）

{{<horizontal_separator>}}

## D問題
https://atcoder.jp/contests/abc163/tasks/abc163_d

### 問題概要
数列 \(\{10^{100}, 10^{100}+1, ... , 10^{100}+N\}\) のうち \(K\) 個以上の数を選ぶとき，
それらの和としてとり得る数の個数を\(\mod(10^9+7)\) で出力せよ．

### 制約
- \(1 \leq N \leq 2 \times 10^5\)
- \(1 \leq K \leq N+1\)

### 解答
各数は \(10^{100}\) 以上であるから，選んだ数の個数が異なる場合，和が重複することはない．

\(M\) 個の数を選んだとき，とり得る数 \(num\) は
\[M \times 10^{100} + \displaystyle\sum_{i=0}^{M-1}i \leq num \leq M \times 10^{100} + \displaystyle\sum_{j=N-M+1}^{N}j\]
であり，この範囲内のすべての整数をとり得る．

よって，\(M\) 個の数を選んだとき，とり得る数の個数は，

\[
	\begin{align}
\displaystyle\sum_{j=N-M+1}^{N}j - \displaystyle\sum_{i=0}^{M-1}i +1 &= \dfrac{\{N+(N-M+1)\}M}{2} - \dfrac{(M-1+0)M}{2} + 1 \\
 &= \dfrac{2NM}{2} - \dfrac{2(M-1)M}{2} +1 \\
 &= -M^2 + (N-1)M + 1
\end{align}
\]

これを \(K \leq M \leq N+1\) について全探索して総和をとる．

https://atcoder.jp/contests/abc163/submissions/12126770

## E問題
https://atcoder.jp/contests/abc163/tasks/abc163_e

後日解いたやつ．DPの世界は広いなあというかんじ

### 問題概要
活発度 \(A_i\) を持つ \(N\) 人の幼児が左右一列に並んでいる．
左から \(x\) 番目に並んでいる幼児が左から \(y\) 番目に移動するとき，うれしさが \(A_x \times |x-y|\) だけ生じる．

幼児を一度だけ並び替えさせるとき，うれしさの総和の最大値を出力せよ．

### 制約
- \(2 \leq N \leq 2000\)
- \(1 \leq A_i \leq 10^9\)

### 解答
活発度 \(A_i\) が大きい幼児を端に優先的に配置していく．
DPの添字を以下のように定める．

- \(dp[l][r] =\) (左端には幼児が \(l\) 番目の位置まで，右端には幼児が \(r\) 番目の位置まで配置されたときのうれしさの最大値)

![dp[i][r]イメージ](image.png)

```cpp
int N;
pqueue<pair<ll,int>> q;
ll dp[2002][2002];
ll res=0;

void input() {
	cin>>N;
	for(int i=1; i<=N; i++) {
		ll a; cin>>a;
		q.push({a,i});
	}
}

void solve() {
	for(int d=N+1; d>1; d--) { // d=l-r  d=1のときすべての幼児が配置される
		ll A=q.top().first;
		int pos=q.top().second;
		q.pop();
		for(int l=0; l+d<=N+1; l++) {
			int r=l+d;
			chmax(dp[l+1][r],dp[l][r]+A*abs(l+1-pos));
			chmax(dp[l][r-1],dp[l][r]+A*abs(r-1-pos));
		}
	}
	for(int i=0; i<=N; i++) { // d=l-r=1の状態をすべて確認し，最大値を取得する
		chmax(res,dp[i][i+1]);
	}
	cout<<res<<endl;
}
```

https://atcoder.jp/contests/abc163/submissions/12511768
