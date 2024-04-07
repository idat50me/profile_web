---
title: "set (+ priority_queue) に自作の比較関数を設定する方法と罠"
date: 2023-10-01T01:13:02+09:00
showLastmod: true
draft: false
toc: false
math: true
images:
---

vector の比較関数と同じように設定したかったので、いろいろ試行錯誤した結果の話

{{<horizontal_separator>}}

## 逆順ソートしたい場合
vector と同じく、`greater<T>`を設定してあげるだけでいいです。

```cpp
set<int, greater<int>> s = {1, 2, 3, 4, 5};
for(int x : s) cout << x << " ";
cout << endl;
// output: 5 4 3 2 1 
```

## ラムダ式で比較関数を設定する
今回の主目的はこちら

要素そのもので比較する場合はもちろん、外部の配列の情報を使って比較することもできます。
今回はある要素の添字のみをソートすることを考えます。

```cpp
vector<int> v = {0, 5, 3, 7, 8, 2};
auto op = [&v](int l, int r) { return v[l] < v[r]; };
```

このように、要素を添字とみなすことで配列の値によって比較を行うこともできます。

```cpp
set<int, decltype(op)> s({1, 2, 3, 4, 5}, op);
for(int x : s) cout << x << " ";
cout << endl;
// output: 5 2 1 3 4 
```

`decltype(op)` 書くのがめんどくさい人はマクロを利用して代替できます。

```cpp
#define set_op(T, op, ...) (set<T, decltype(op)>(__VA_OPT__(__VA_ARGS__, ) op))

// 略

auto s = set_op(int, op, {1, 2, 3, 4, 5});
for(int x : s) cout << x << " ";
cout << endl;
```

~~あんまり書く量変わってないね~~

### 比較関数の罠
こちらをご覧ください。（`v` を変えただけです）
```cpp
vector<int> v = {0, 0, 0, 0, 0, 0}; // すべて 0 に変更
auto op = [&v](int l, int r) { return v[l] < v[r]; };

set<int, decltype(op)> s({1, 2, 3, 4, 5}, op);
for(int x : s) cout << x << " ";
cout << endl;
// output: 1 
```

`s`に \(1\) しか入ってないですね。

`op`は set の重複判定にもが使われます。
`s`に入っている要素自体が異なっていても、`op` の判定結果が同値ならば同じ要素とみなされます．

```cpp
vector<int> v = {0, 0, 0, 0, 0, 0};
// v[l] と v[r] が等しい場合でも優先度を決めておく
auto op = [&v](int l, int r) { return v[l] == v[r] ? l < r : v[l] < v[r]; };

set<int, decltype(op)> s({1, 2, 3, 4, 5}, op);
for(int x : s) cout << x << " ";
cout << endl;
// output: 1 2 3 4 5 
```

順番どっちでもいいんだよな～～という場合でも、しっかりユニークな情報で場合分けしておきましょう。

## priority_queue の場合
基本的に set と同じでいいんですが、

```cpp
vector<int> v = {0, 5, 3, 7, 8, 2};
auto op = [&v](int l, int r) { return v[l] < v[r]; };

priority_queue<int, vector<int>, decltype(op)> q(op);
for(int x : {1, 2, 3, 4, 5}) q.push(x);
while(not q.empty()) {
	cout << q.top() << " ";
	q.pop();
}
cout << endl;
// output: 4 3 1 2 5 
```

vector や set と逆順でソートされます。
もちろん`op`の不等号を逆にしてあげればいつも通りになります。

## この記事を書いた動機
ABC314_G を解くときにいろいろ調べて面白かったので。

https://atcoder.jp/contests/abc314/submissions/46047150
