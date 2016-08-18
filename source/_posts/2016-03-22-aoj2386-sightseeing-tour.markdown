---
layout: post
title: "AOJ2386 Sightseeing Tour"
date: 2016-03-22 18:40:06 +0900
comments: true
categories: [AOJ-ICPC, 250, グラフ]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-type="article" data-card-branding="0"><h4><a href="http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=2386">Sightseeing Tour | Aizu Online Judge</a></h4><p>Introduction to Programming Introduction to Algorithms and Data Structures Library of Data Structures Library of Graph Algorithms Library of Computational Geometry Library of Dynamic Programming Library of Number Theory</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

隣接グラフが与えれる．全ての{% m %}2{% em %}点間を結ぶコストが最小であり，ハミルトン路を持つグラフのコストの和を答える．  
完全グラフは向きにかかわらずに必ずハミルトン路を持つらしいので， {% m %} 2 {% em %}点間のコストの小さい方を採用する．

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

int main() {
	int n;
	cin >> n;

	ll cost[105][105];
	memset(cost, 0, sizeof(cost));

	rep(i, n) {
		rep(j, n) cin >> cost[i][j];
	}

	ll ans = 0;
	rep(i, n) {
		REP(j, i+1, n) {
			ans += min(cost[i][j], cost[j][i]);
		}
	}

	cout << ans << endl;

	return 0;
}
```

