---
title: "ABC167"
date: 2020-05-11T17:13:41+09:00
showLastmod: true
draft: false
toc: false
math: true
images:
---

## C問題
https://atcoder.jp/contests/abc167/tasks/abc167_c

### 問題概要
学習するアルゴリズムが \(M\) 個，参考書が \(N\) 冊ある．
\(i \) 番目の参考書は \(C_i \) 円で売られており，\(j \, ( 1 \leq j \leq M ) \) 番目のアルゴリズムの理解度が \(A_{ij} \) 上がる．

すべてのアルゴリズムの理解度を \(X \) 以上にするために必要な金額を求めよ．

### 制約
- \(1 \leq N,M \leq 12 \)
- \(1 \leq X \leq 10^5 \)
- \(1 \leq C_i \leq 10^5 \)
- \(0 \leq A_{ij} \leq 10^5 \)

### 解答
買う参考書を全探索して，理解度の条件を満たすとき必要な金額を更新する．

結構重いbit全探索だった気がするけど茶下位diffだった．

```cpp
int N,M,X;
int C[12];
int A[12][12];
int res=INTINF;
 
void input() {
	cin>>N>>M>>X;
	for(int i=0; i<N; i++) {
		cin>>C[i];
		for(int j=0; j<M; j++) cin>>A[i][j];
	}
}
 
bool isok(intvec score) {
	for(int n: score) {
		if(n<X) return false;
	}
	return true;
}
 
void solve() {
	for(int bit=0; bit<(1<<N); bit++) {
		int m=0;
		intvec score(M,0);
		for(int j=0; j<N; j++) {
			if(!(bit & (1<<j))) continue;
			m+=C[j];
			for(int k=0; k<M; k++) score[k]+=A[j][k];
		}
		if(isok(score)) {
			chmin(res,m);
		}
	}
 
	cout<<(res==INTINF ? -1 : res)<<endl;
}
```

https://atcoder.jp/contests/abc167/submissions/13045232


## D問題
https://atcoder.jp/contests/abc167/tasks/abc167_d

### 問題概要
町が \(N \) 個あり，それぞれの町にはテレポーターが設置されている．
町 \(i\, ( 1 \leq i \leq N ) \) のテレポーターの転送先は町 \(A_i \) である．

町 \(1\) から出発し，テレポーターを \(K\) 回使用したときに到着する町を出力せよ．

### 制約
- \(2 \leq N \leq 2 \times 10^5 \)
- \(1 \leq A_i \leq N \)
- \(1 \leq K \leq 10^{18} \)

### 解答
各町に到着した時刻を記録しておく．
2回目の到着（＝ループ発生）の時刻と1回目の到着時刻から，1回のループにかかる時間を計算できる．

以降，残りの転送回数を1回のループにかかる時間で割った剰余だけシミュレートすればよい．

- `times[i]`：\(i\) 番目の町に最初に到着した時刻
- `town`：現在のいる町
- `tm`：現在時刻
- `lp`：1回のループにかかる時間
- `k`：ループ後の残り転送回数

```cpp
int N;
ll K;
intvec A;
intvec times;
 
void input() {
	cin>>N>>K;
	init(A,N);
	for(int i=0; i<N; i++) A[i]--;
	times.resize(N,-1);
}
 
void solve() {
	int town=0, tm=0;
	while(times[town]==-1 && tm<K) {
		times[town]=tm;
		town=A[town];
		tm++;
	}
	int lp=tm-times[town];
	int k=(K-tm)%lp;
	for(int i=0; i<k; i++) town=A[town];
	cout<<town+1<<endl;
}
```

これも茶diffなのか...

https://atcoder.jp/contests/abc167/submissions/13052655


## E問題
https://atcoder.jp/contests/abc167/tasks/abc167_e

### 問題概要
一列に並んだ \(N\) ブロックを \(M\) 種類の色で塗ることを考える．

隣り合うブロック同士が同じ色で塗られている組を \(K\) 組以下にするとき，ブロック列の塗り方が何通りあるか求めよ．使わない色があってもよい．

### 制約
- \(1 \leq N,M \leq 2 \times 10^5 \)
- \(0 \leq K \leq N-1 \)

### 解答
左から \(n\) ブロックまでで，隣同士同じ色の組が \(k\) 組あると考える．

\(n=1\) のとき，1ブロック目に \(M\) 種類の色を塗ることができるので，塗り方は \(M\) 通り．

\(n=2, k=0\) のとき， \(n=1\) の1ブロック目の色と異なる色で塗ればいいので，1通りあたり \(M - 1\) 個の分岐がある．
よって \(M(M - 1)\) 通り．

\(n=2,k=1\)のとき， 1ブロック目と同じ色で塗ればいいので，1通りあたり分岐は1つのみ．
よって \(M\) 通り．

\(n=3,k=0\) のとき，\(n=2,k=0\) から1通りあたり \(M - 1\) 個の分岐がある．よって \(M(M - 1)^2\) 通り．

\(n=3,k=1\) のとき，\(n=2,k=0\) から分岐する場合と \(n=2,k=1\) から分岐する場合がある．

- \(n=2,k=0\) からの分岐は，1通りあたり1つの分岐があるので \(M(M - 1)\) 通り
- \(n=2,k=1\) からの分岐は，1通りあたり \(M - 1\) 個の分岐があるので \(M(M - 1) \) 通り

よって \(2M(M- 1)\) 通り．

\(n=3,k=2 \) の場合は，\(n=2,k=1 \) からの1通りあたり1つの分岐しかないので \(M\) 通り．

ここまでくるとDPの遷移が見えてくる．

$$
dp[n][k] = 
\begin{cases}
dp[n-1][k] \times (M - 1) & (k=0) \\\\
dp[n-1][k] \times (M - 1) + dp[n-1][k - 1] & (1 \leq k \leq n-2) \\\\
dp[n-1][k - 1] & (k=n-1)
\end{cases}
$$

これは二項係数であるから，

$$ dp[n][k] = {}_{n-1}C_k M (M - 1)^{n-1-k} $$

以上より，答えは\(\displaystyle\sum_{k=0}^M {}_{N-1}C_k M (M - 1)^{N-1-k} \)

https://atcoder.jp/contests/abc167/submissions/13065438

二項係数の剰余を求めるアルゴリズムは[kamiさんの記事](https://algo-logic.info/combination/)，[けんちょんさんの記事](https://drken1215.hatenablog.com/entry/2018/06/08/210000)がわかりやすかった．
