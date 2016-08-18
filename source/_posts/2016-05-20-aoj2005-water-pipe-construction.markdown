---
layout: post
title: "AOJ2005 Water Pipe Construction"
date: 2016-05-20 23:21:05 +0900
comments: true
categories: [AOJ-ICPC, 350, グラフ]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-branding="0" data-card-type="article"><h4><a href="http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=2005">Water Pipe Construction | Aizu Online Judge</a></h4><p>Introduction to Programming Introduction to Algorithms and Data Structures Library of Data Structures Library of Graph Algorithms Library of Computational Geometry Library of Dynamic Programming Library of Number Theory</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

水源{% m %} s {% em %}から， {% m %} 2つの {% em %}主要な基地 {% m %} g_1, g_2 {% em %}まで水を運ぶための導水管の敷設にかかるコストを最小化する．  
以下の{% m %} 3 {% em %}パターンに分かれる．  

{% img /images/AOJ/2005-1.png %}
{% img /images/AOJ/2005-2.png %}
{% img /images/AOJ/2005-3.png %}
  
　{% m %} n(3 \leq n \leq 100) {% em %}なのでwarshall-floydで最短経路を出してこの {% m %} 3 {% em %}パターンのminを取った．  
経路復元がいるかと思ったけど，いらなかった．もし {% m %} s \to g1 \to g2 {% em %}で {% m %} s \to g1 {% em %}と {% m %} g1 \to g2 {% em %}の通る頂点に重なりがある場合，重複して道を数えているのでそのパターンではなく，パターン {% m %} 3 {% em %}のケースの方が良い．

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

ll d[105][105];
int to[105][105];

void warshall_floyd(int n, int m) {
	rep(i, n) rep(j, n) d[i][j] = INF;
	rep(i, n) d[i][i] = 0;

	rep(i, n) rep(j, n) to[i][j] = j;

	//input
	rep(i, m) {
		int a, b, c;
		cin >> a >> b >> c;

		a--; b--;
		d[a][b] = c;
		// d[b][a] = c;
	}

	rep(k,n) {
		rep(i,n) {
			rep(j,n) {
				if(d[i][k] == INF || d[k][j] == INF) continue;
				if(d[i][j] > d[i][k] + d[k][j]) {
					d[i][j] = d[i][k] + d[k][j];
					to[i][j] = to[i][k];
				}
			}
		}
	}
}

vector<int> path(int s, int g) {
	int cur = s;
	vector<int> ret;
	for(; cur != g; cur = to[cur][g]) {
		ret.push_back(cur);
	}

	ret.push_back(g);
	return ret;
}

int main() {
	int n, m, s, g1, g2;
	while(cin >> n >> m >> s >> g1 >> g2) {
		if(n == 0 && m == 0 && s == 0 && g1 == 0 && g2 == 0) break;

		s--; g1--; g2--;
		warshall_floyd(n, m);

		ll ans = INF;
		rep(i, n) {
			ans = min(ans, d[s][i] + d[i][g1] + d[i][g2]);
		}

		ans = min(ans, d[s][g1] + d[g1][g2]);
		ans = min(ans, d[s][g2] + d[g2][g1]);

		cout << ans << endl;
	}

	return 0;
}
```

