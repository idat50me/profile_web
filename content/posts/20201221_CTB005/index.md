---
title: "CafeCoder Tea Break 005"
date: 2020-12-21T18:37:25+09:00
showLastmod: true
draft: false
toc: false
math: true
---

全完できたな～と思って書いてたら5完1WAだった

{{<horizontal_separator>}}

## A問題
https://mofecoder.com/contests/tea005/tasks/tea005_a

### 問題概要
整数 \(N\) が与えられる．また変数 \(X,Y\) があり，これらは最初 \(0\) である．
「\(X\) と \(N\) の bit ごとの論理和」と「\(Y\) と \(N\) の bit ごとの排他的論理和」を求めよ．

### 制約
- \(1 \leq N \leq 10\)

### 解答
\(X,Y\) を更新する操作はないため，\(0\) と \(N\) の bit ごとの論理和・排他的論理和を求める問題に等しい．
ヒントを読むと，どちらも \(N\) になることがわかる．

https://mofecoder.com/contests/tea005/submissions/2562

## B問題
https://mofecoder.com/contests/tea005/tasks/tea005_b

### 問題概要
長さ \(N\) の文字列 \(S,T\) が与えられる．

以下の操作を行うことができる．
- 文字列 \(S\) のうち1文字をアルファベット順で1つ前または1つ後ろの文字で置き換える．ただし `z` と `a` はアルファベット順でつながっているものとする．

\(S\) と \(T\) を等しくするために必要な操作回数の最小値を求めよ．

### 制約
- \(1 \leq N \leq 10^{5}\)
- \(|S| = |T| = N\)
- \(S,T\) は英小文字からなる文字列

### 解答
\(S\) の各文字について，アルファベット順で前にずらしていく操作と後ろにずらしていく操作で，操作回数が少ないほうを選んでいくとよい．

https://mofecoder.com/contests/tea005/submissions/2600

## C問題
https://mofecoder.com/contests/tea005/tasks/tea005_c

### 問題概要
正整数 \(A,B,C\) が与えられる．

\(\dfrac{Ax}{B} \geq C\) を満たす最小の整数 \(x\) を求めよ．

### 制約
- \(1 \leq A,B,C \leq 10^{9}\)

### 解答
$$
\frac{Ax}{B} \geq C
\Longleftrightarrow x \geq \frac{BC}{A}
$$

のように式を変形できる．

C++の場合，\(\dfrac{a}{b}\) 以上の整数（切り上げ）は
```cpp
ans = (a+b-1) / b;
```
とすることで求めることができる．

https://mofecoder.com/contests/tea005/submissions/2622

## D問題
https://mofecoder.com/contests/tea005/tasks/tea005_d

### 問題概要
\(n\) 個の整数 \(a_1, a_2, ... , a_n\) が与えられる．

\(\displaystyle\prod_{i=1}^{n} a_i! \mod 10^9+7\) を求めよ．

### 制約
- \(1 \leq n \leq 2 \times 10^{5}\)
- \(1 \leq a_i \leq 2 \times 10^{5}\)

### 解答
与えられた整数が \(9, 5, 6, 2\) の場合，\(10^{9}+7\) で割る前の答えは

$$9! \cdot 5! \cdot 6! \cdot 2! = 1^{4} \cdot 2^{4} \cdot 3^{3} \cdot 4^{3} \cdot 5^{3} \cdot 6^{2} \cdot 7^{1} \cdot 8^{1} \cdot 9^{1}$$

となる．これを見てみると，与えられた各整数を境に指数が減っていくことがわかる．

したがって，与えられた整数をソートして，現在の指数を保持しながら乗算していき，
与えられた整数の累乗計算を終えたら指数の値を減らしていけばよい．

\(a^{m}\) の計算を愚直に行うと \(O(m)\) かかるが，繰り返し二乗法を用いることで \(O(\log m)\) で求めることができる．
よって，全体で \(O(\max(a_i) \log n)\) で求まる．

https://mofecoder.com/contests/tea005/submissions/2680

## E問題
https://mofecoder.com/contests/tea005/tasks/tea005_e

### 問題概要
\(2^{(a^{b})} \mod k\) を求めよ．

### 制約
- \(1 \leq a,b,k \leq 10^{9}+7\)
- \(k\) は素数

### 解答
フェルマーの小定理を用いる．（「mod 累乗」等で調べると辿り着けそう）
https://ja.wikipedia.org/wiki/%E3%83%95%E3%82%A7%E3%83%AB%E3%83%9E%E3%83%BC%E3%81%AE%E5%B0%8F%E5%AE%9A%E7%90%86

> \(p\) が素数であり，\(a\) と \(p\) が互いに素であるとき，
> $$ a^{p-1} \equiv 1\ \ \ \ (\bmod\ p) $$
> が成り立つ．

したがって，\(a^b = n(k-1) + m\) としたとき，
$$
2^{(a^{b})} = 2^{n(k-1)} \cdot 2^m \equiv 2^m\ \ \ \ (\bmod\ k)
$$
である．

\(k=2\) のときに限り，「\(a\) と \(p\) が互いに素である」というフェルマーの小定理の成立条件を満たさないため，例外処理が必要である．
\(a,b \geq 1\) より，\(2^{(a^{b})}\) は必ず \(2\) の倍数であるから，\(\bmod\ 2\) は必ず \(0\) である．

https://mofecoder.com/contests/tea005/submissions/2977

## F問題
https://mofecoder.com/contests/tea005/tasks/tea005_f

### 問題概要
長さ \(N\) の数列 \(A_1, A_2 , ... , A_N\) が与えられる．
また，\(0\) で初期化された変数 \(x\) がある．

\(1 \leq i \leq N\) について，\(i\) が小さい順に以下のどちらかの操作を行う．
- \(x\) を「\(x\) と \(A_i\) の bit ごとの論理和」で置き換える
- \(x\) を「\(x\) と \(A_i\) の bit ごとの排他的論理和」で置き換える

\(N\) 回操作した後の \(2^{N}\) 通りの状態について，\(x\) の総和を求めよ．

### 制約
- \(1 \leq N \leq 2 \times 10^{5}\)
- \(0 \leq A_i \leq 10^{18}\)

### 解答
各 bit の桁ごとに以下の DP を考える．

\(dp[i][j] =\) \(i\) 回操作を終えた時点で，bit が \(j\) である場合の数

として，\(N\) 回の操作を終えた後に bit が \(0\) になる場合の数，\(1\) になる場合の数を求めることができる．


\(\displaystyle\sum_{k=0}^{59}\) (\(N\) 回操作した時点で \(k\) 桁目が \(1\) である場合の数) \(\times 2^{k}\)
が答えとなる．

https://mofecoder.com/contests/tea005/submissions/2771
