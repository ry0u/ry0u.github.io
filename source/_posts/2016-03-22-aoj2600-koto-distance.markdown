---
layout: post
title: "AOJ2600 Koto Distance"
date: 2016-03-22 20:29:30 +0900
comments: true
categories: [AOJ-ICPC, 250, 累積和]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-type="article" data-card-branding="0"><h4><a href="http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=2600">Koto Distance | Aizu Online Judge</a></h4><p>Introduction to Programming Introduction to Algorithms and Data Structures Library of Data Structures Library of Graph Algorithms Library of Computational Geometry Library of Dynamic Programming Library of Number Theory</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

X方向，Y方向それぞれに累積和を取る．X，Yのどちらかが埋まっていれば覆われている．

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

#define REP(i,k,n) for(int i=k;i<n;i++)
#define rep(i,n) for(int i=0;i<n;i++)
#define INF 1<<30
#define pb push_back
#define mp make_pair

using namespace std;
typedef long long ll;
typedef pair<int,int> P;

int X[100005], Y[100005];

int main() {
	int n, W, H;
	cin >> n >> W >> H;

	memset(X, 0, sizeof(X));
	memset(Y, 0, sizeof(Y));

	rep(i, n) {
		int x, y, w;
		cin >> x >> y >> w;

		X[max(0, x-w)]++;
		X[min(W, x+w)]--;

		Y[max(0, y-w)]++;
		Y[min(H, y+w)]--;
	}

	REP(i, 1, 100005) {
		X[i] += X[i-1];
		Y[i] += Y[i-1];
	}

	bool a = true, b = true;
	REP(i, 0, W) {
		if(X[i] == 0) a = false;
	}

	REP(i, 0, H) {
		if(Y[i] == 0) b = false;
	}

	if(a || b) cout << "Yes" << endl;
	else cout << "No" << endl;

	return 0;
}
```

