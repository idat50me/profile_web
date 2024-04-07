---
title: "Educational Codeforces Round 83 E. Array Shrinking"
date: 2020-04-24T18:13:23+09:00
showLastmod: true
draft: false
toc: false
math: true
images:
---

区間DPによる数列の圧縮
そのうち忘れそうなので書いておく

### 問題概要
長さ \(N\) の数列 \(A=\{a_1,a_2,...,a_N\}\) について以下の操作を0回以上行う．

- 隣接する2つの値が等しいとき（値を \(v\) とする），2つの値を消去し，代わりに \(v+1\) を挿入する．

操作を終えたときの数列の長さの最小値を出力する．

### 制約
- \(1 \leq N \leq 500\)
- \(1 \leq a_i \leq 1000\)

### 解答
下記コードの通り．
\(val\) は \(dp\) が \(1\) の場合の値．

```cpp
int N;
llvec a;
int val[501][501];
int dp[501][501];

void input() {
	cin>>N;
	a.resize(N);
	for(int i=0; i<N; i++) {
		cin>>a[i];
	}
	for(int i=0; i<=N; i++) {
		for(int j=0; j<=N; j++) {
			dp[i][j]=INTINF;
			val[i][j]=-1;
		}
		if(i+1<=N) {
			dp[i][i+1]=1;
			val[i][i+1]=a[i];
		}
	}
}

void solve() {
	for(int d=2; d<=N; d++) { // 区間長さを徐々に伸ばしていく
		for(int l=0; l+d<=N; l++) {
			int r=l+d;
			for(int mid=l+1; mid<r; mid++) {
				// [l,r)に値が2つしかない && 2値が等しければ結合
				// そうじゃなければなにもしない
				if(dp[l][mid]==1 && dp[mid][r]==1 && val[l][mid]==val[mid][r]) {
					dp[l][r]=1;
					val[l][r]=val[l][mid]+1;
				}
				else {
					chmin(dp[l][r],dp[l][mid]+dp[mid][r]);
				}
			}
		}
	}

	cout<<dp[0][N]<<endl;
}
```

#### 例
\(A=\{1,1,2,2\}\) のとき，
- \(dp[0][1]=1, val[0][1]=1\)
- \(dp[1][2]=1, val[1][2]=1\)
- \(dp[2][3]=1, val[2][3]=2\)
- \(dp[3][4]=1, val[3][4]=2\)

{{<br>}}

- \(dp[0][2]=1, val[0][2]=val[0][1]+1=2\)
- \(dp[1][3]=2\)
- \(dp[2][4]=1, val[2][4]=val[2][3]+1=3\)

{{<br>}}

- \(dp[0][3]=dp[0][2]+dp[2][3]=2\)
- \(dp[1][4]=dp[1][2]+dp[2][4]=2\)

{{<br>}}

- \(dp[0][4]=dp[0][2]+dp[2][4]=2\)

https://codeforces.com/contest/1312/problem/E

