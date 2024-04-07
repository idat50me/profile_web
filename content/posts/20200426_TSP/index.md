---
title: "巡回セールスマン問題"
date: 2020-04-26T15:32:16+09:00
showLastmod: true
draft: false
toc: false
math: true
images:
---

bitDP

{{<horizontal_separator>}}

巡回セールスマン問題をDPで求めるときは引数が2つになる．

- \(bit =\) (巡回済の点のビットフラグ)
- \(dp[bit][now] =\) (現在点が \(now\) で巡回済が \(bit\) の時の最小コスト)


\(bit = 11...11\) となる最小コストを求める．最後に到達する点はどこでもいいので，`solve`関数内で最終到達点を全探索している．

```cpp
int N;
int dist[20][20];
int dp[(1<<20)+1][20];
int res=INTINF;

void input() {
	cin>>N;
	for(int i=0; i<N; i++)for(int j=0; j<N; j++) cin>>dist[i][j];
	for(int i=1; i<(1<<N); i++)for(int j=0; j<N+1; j++) dp[i][j]=-1;
}

int rec(int bit, int now) {
	if(dp[bit][now]!=-1) return dp[bit][now]; // 計算済ならそのまま
	if(bit==(1<<now)) return dp[bit][now]=0; // 最初の点だったらコストは0だよね

	int tmp=INTINF;
	int pbit=bit & ~(1<<now); // nowの前の点としてありえるやつ

	for(int prev=0; prev<N; prev++) {
		if(pbit & (1<<prev)) {
			chmin(tmp,rec(pbit,prev)+dist[prev][now]); // nowの前の点をprevとする
		}
	}

	return dp[bit][now]=tmp;
}

void solve() {
	for(int i=0; i<N; i++) {
		chmin(res, rec((1<<N)-1,i)); // 最後の点をどこにするかでfor回す
	}
	cout<<res<<endl;
}
```

{{<horizontal_separator>}}

bitDPを気軽に使える足掛かりにしたい
