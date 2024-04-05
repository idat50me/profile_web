---
title: 'ABC154'
date: 2020-03-29T17:43:45+09:00
draft: false
toc: false
math: true
images:
---


## C問題
### 問題概要
整数列 \(A_1,A_2,...,A_N\) のどの2つの要素も互いに異なるならば`YES`，そうでなければ`NO`を出力．

### 制約
- \(2 \leq N \leq 200000\)
- \(1 \leq A_i \leq 10^9\)

### 解答
\(N\) の制約から，各入力毎にそれまでの入力と異なるか比較するとTLEとなるが，無理やりにぶたん使おうとして2WAしている．[^c1]
[^c1]:にぶたんの関数を作ったばかりで使いたがりだった模様

実際は整数列をソートして隣接する整数を貪欲に比較するだけでよい．

https://atcoder.jp/contests/abc154/tasks/abc154_c
https://atcoder.jp/contests/abc154/submissions/10001131


## D問題
### 問題概要
\(N\) 個のサイコロが一列に並べられており，\(i\) 番目のサイコロは \(1\) から \(p_i\) までの目が等確率で出る．
隣接する \(K\) 個のサイコロを振ったときの出る目の総和の期待値の最大値を出力．

### 制約
- \(1 \leq K \leq N \leq 200000\) 
- \(1 \leq p_i \leq 1000\) 

### 解答
**隣接する** \(K\) 個を選ばなければならないので，\(p_i\) をソートして上から選択はだめ．

最初の \(K\) 個の総和 \(\Sigma_K p_i\) を`tmpsum`,`maxsum`に代入する．
以降は入力毎に`tmpsum`,`maxsum`を更新する．

```cpp
for(int i=0; i<k; i++) {
    cin>>p[i];
    maxsum+=p[i];
}

tmpsum=maxsum;
for(int i=k; i<n; i++) {
    cin>>p[i];
    tmpsum+=p[i]-p[i-k];
    chmax(maxsum,tmpsum);
}
```

選択したサイコロの目の最大値を \(q_1,q_2,...,q_K\) とすると，総和の期待値は
\(\dfrac{q_1+1}{2} + \dfrac{q_2+1}{2} + \cdots + \dfrac{q_K+1}{2} = \dfrac{\Sigma_K q_i + K}{2}\)
である．

```cpp
cout<<(maxsum+k)/2.0<<endl;
```

ところが小数点以下の桁が増えると浮動小数点表示になる（2WA）ため，
```cpp
cout<<fixed<<setprecision(12)<<(maxsum+k)/2.0<<endl;
printf("%.12f\n",(maxsum+k)/2.0);
```
とすれば固定小数点表示になりAC．[^d1]
[^d1]:printfのほうが使いやすそう

https://atcoder.jp/contests/abc154/tasks/abc154_d
https://atcoder.jp/contests/abc154/submissions/10000089

## E問題
### 問題概要
\(1\) 以上 \(N\) 以下の整数で，10進数で表したとき \(0\) 以外の数字が \(K\) 個ある数の個数を出力．

### 制約
- \(1 \leq N \leq 10^{100}\)
- \(1 \leq K \leq 3\)

### 解答
コンテスト後に提出．

桁DPを用いる．パラメータは確認した桁数，\(N\) 未満フラグ，非0の桁数とする．

`dp[0][0][0]=1`，それ以外の要素について`dp[i][j][k]=0`と初期化して各i,j,kについてforループをぶんまわす．

```cpp
for(int i=0; i<n.length(); i++) {
	int D=n.at(i)-'0';
	for(int j=0; j<2; j++)
		for(int k=0; k<=i; k++) {
			for(int d=0; d<=(j?9:D); d++) {
				if(d!=0) dp[i+1][j||(d!=D)][k+1] += dp[i][j][k];
				else     dp[i+1][j||(d!=D)][k] += dp[i][j][k];
			}
		}
}
```
`d`は次の桁の数字であり， \(N\) 未満フラグ`j`が`true`すなわち \(N\) 未満が既に確定しているならば，`d`はどの数字をとってもよい．
`false`すなわち \(N\) 未満が確定していない((確定している桁までNと一致していればfalseである))ならば，`d`は \(N\) を超えるような数字をとることはできない．

```cpp
cout<<dp[n.length()][0][K]+dp[n.length()][1][K]<<endl;
```
すべての桁が確定したとき， \(N\) 未満フラグが`false`のケースは \(N\) のケースのみであるから，
条件が「 \(1\) 以上 \(N\) 未満」ならば \(N\) 未満フラグが`true`の場合のみを出力すればよい．

https://atcoder.jp/contests/abc154/tasks/abc154_e
https://atcoder.jp/contests/abc154/submissions/10944107

## まとめ
たぶん備忘録続かないなあと思いつつのんびりやる
