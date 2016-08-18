---
layout: post
title: "AOJ1175 And Then. How Many Any There?"
date: 2016-05-21 01:35:44 +0900
comments: true
categories: [AOJ-ICPC, 350, 全探索]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-branding="0" data-card-type="article"><h4><a href="http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=1175&lang=jp">And Then. How Many Are There? | Aizu Online Judge</a></h4><p>Introduction to Programming Introduction to Algorithms and Data Structures Library of Data Structures Library of Graph Algorithms Library of Computational Geometry Library of Dynamic Programming Library of Number Theory</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

別の円盤が上に無い状態の円盤は同じ色が偶数の場合は全部消し，奇数の場合は {% m %} 1 {% em %}個消さないのを選ぶ，というやり方でやっていたが，消す順番によりそれより多く消せるパターンというのがありこれは間違いだった．  
選ぶ順番によって消せる枚数が違ってくるので，各状態毎に {% m %} (i, j) {% em %}を列挙して消せるかどうかを調べる． {% m %} n {% em %}は最大で {% m %} 4 * 6 = 24 {% em %}なので，状態数は {% m %} 2 ^{24} {% em %}．intのbitでどの円盤を使ったかを管理した． {% m %} O(2 ^n n ^2)  {% em %}．

# Code

```cpp
#include <iostream>
#include <vector>
#include <string>
#include <cstring>
#include <algorithm>
#include <sstream>
#include <map>
#include <set>
#include <cmath>

#define REP(i,k,n) for(int i=k;i<n;i++)
#define rep(i,n) for(int i=0;i<n;i++)
#define INF 1<<30
#define pb push_back
#define mp make_pair

using namespace std;
typedef long long ll;

struct P {
	int x, y, r, c;

	P() : x(0), y(0), r(0), c(0) {}
	P(int x, int y, int r, int c) : x(x), y(y), r(r), c(c) {}
};

int n, ans;
vector<P> v;
set<int> memo;

void dfs(int S) {
	if(memo.find(S) != memo.end()) return;
	memo.insert(S);

	ans = max(ans, __builtin_popcount(S));
	if(ans == n) return;

	bool up[50];
	memset(up, 0, sizeof(up));
	for(int i = n-1; i >= 0; i--) {
		if(S & (1<<i)) continue;

		bool flag = true;
		for(int j = i-1; j >= 0; j--) {
			if(S & (1<<j)) continue;
			int dist = (v[j].x - v[i].x) * (v[j].x - v[i].x) + (v[j].y - v[i].y) * (v[j].y - v[i].y);
			int len = (v[i].r + v[j].r) * (v[i].r + v[j].r);

			if(dist < len) {
				flag = false;
			}
		}

		if(!flag) up[i] = true;
	}

	set<int> st[5];
	for(int i = n-1; i >= 0; i--) {
		if((S & (1<<i)) || up[i]) continue;

		bool flag = true;

		for(int j = i-1; j >= 0; j--) {
			if((S & (1<<j)) || up[j]) continue;

			int dist = (v[j].x - v[i].x) * (v[j].x - v[i].x) + (v[j].y - v[i].y) * (v[j].y - v[i].y);
			int len = (v[i].r + v[j].r) * (v[i].r + v[j].r);

			if(dist >= len && v[i].c == v[j].c) {
				dfs(S + (1<<i) + (1<<j));
			}
		}
	}
}

int main() {
	while(cin >> n && n) {

		v.clear();
		memo.clear();

		rep(i, n) {
			int x, y, r, c;
			cin >> x >> y >> r >> c;
			v.push_back(P(x, y, r, c));
		}

		vector<bool> used(n);
		rep(i, n) used[i] = false;

		ans = 0;
		int S = 0;
		dfs(S);

		cout << ans << endl;
	}

	return 0;
}
```

