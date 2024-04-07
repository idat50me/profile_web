---
title: "ABC168"
date: 2020-06-14T13:35:35+09:00
showLastmod: true
draft: false
toc: false
math: true
images:
---

## D問題
### 問題概要
\(N\) 個の部屋と \(M\) 本の通路を持つ洞窟がある．部屋 \(1\) には洞窟の入り口がある．
通路 \(i\ (1 \leq i \leq M)\) は部屋 \(A_i , B_i\ (1 \leq A_i,B_i \leq N)\) を双方向につないでいる．

ここで，部屋 \(1\) 以外のすべての部屋に，「次に向かう部屋への道しるべ」を設置したい．

この道しるべに従って移動するとき，どの部屋からスタートしても入り口まで最短で移動できるような道しるべの置き方が
存在する場合は`YES`を出力し，各部屋に置く道しるべが指す部屋番号を出力せよ．
存在しない場合は`NO`を出力せよ．

### 制約
- \(2 \leq N \leq 10^5\)
- \(1 \leq M \leq 2 \times 10^5\)
- \(1 \leq A_i, B_i \leq N\ (1 \leq i \leq M)\)
- \(A_i \neq B_i\ (1 \leq i \leq M)\)
- グラフは連結である

### 解答
部屋 \(1\) から各部屋までの最短距離はBFSで求まる．
逆に，各部屋から部屋 \(1\) までの最短移動経路は，部屋 \(1\) からBFSを行うときの経路に等しい．

したがって，BFSで部屋 \(A\) から部屋 \(B\) に移動するとき，部屋 \(B\) に置く道しるべは \(A\) としてよい．

```cpp
struct Room {
	intvec nxt;
	int mark=-1;
};

int N,M;
Room rm[100001];
 
void input() {
	cin>>N>>M;
	for(int i=0; i<M; i++) {
		int A,B; cin>>A>>B;
		rm[A].nxt.pushb(B);
		rm[B].nxt.pushb(A);
	}
}
 
void solve() {
	queue<intpair> s;
	s.push({1,0});
	while(!s.empty()) {
		int pos=s.front().fi, prv=s.front().se;
		s.pop();
		if(rm[pos].mark>=0) continue;
		rm[pos].mark=prv;
		for(int i=0; i<rm[pos].nxt.size(); i++) {
			int npos=rm[pos].nxt[i];
			s.push({npos,pos});
		}
	}
 
	cout<<"Yes"<<endl;
	for(int i=2; i<=N; i++) {
		cout<<rm[i].mark<<endl;
	}
}
```

各部屋から部屋 \(1\) までの経路は必ず存在するから，条件を満たす道しるべの配置も必ず存在する．
