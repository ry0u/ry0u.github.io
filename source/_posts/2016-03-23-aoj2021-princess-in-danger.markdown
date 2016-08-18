---
layout: post
title: "AOJ2021 Princess in Danger"
date: 2016-03-23 19:48:34 +0900
comments: true
categories: [AOJ-ICPC, 300, グラフ, dijkstra]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-type="article" data-card-branding="0"><h4><a href="http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=2021">Princess in Danger | Aizu Online Judge</a></h4><p>Introduction to Programming Introduction to Algorithms and Data Structures Library of Data Structures Library of Graph Algorithms Library of Computational Geometry Library of Dynamic Programming Library of Number Theory</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

状態を {% m %} (cost, 残り時間, 番号) {% em %}としてdijkstra．現在の状態の残り時間のほうが辺のコストより大きい時に，次の町に行くことが可能．冷凍施設にいるときは， 現在の残り時間から{% m %} M {% em %}まで回復出来るので {% m %} 1 {% em %}分ずつ試す．


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
#include <queue>

#define REP(i,k,n) for(int i=k;i<n;i++)
#define rep(i,n) for(int i=0;i<n;i++)
#define INF 1<<30
#define pb push_back
#define mp make_pair

using namespace std;
typedef long long ll;
typedef pair<int,int> P;
typedef pair<P, int> PI;
typedef pair<P, P > PP;

struct edge {
	int from,to;
	int cost;

	edge(int t,int c) : to(t),cost(c) {}
	edge(int f,int t,int c) : from(f),to(t),cost(c) {}

	bool operator<(const edge &e) const {
		return cost < e.cost;
	}
};

int n, m, l, k, a, h;
vector<edge> G[105];
int d[105][105];
bool L[105];

void dijkstra(int s) {
	priority_queue<PI, vector<PI>, greater<PI> > que;
	rep(i, 105) {
		rep(j, 105) {
			d[i][j] = INF;
		}
	}

	d[s][m] = 0;
	que.push(PI(P(0, m), s));

	while(que.size()) {
		PI p = que.top();
		que.pop();

		int cost = p.first.first;
		int t = p.first.second;
		int v = p.second;
		if(d[v][t] < cost) continue;

		if(L[v]) {
			REP(i, t+1, m+1) {
				if(d[v][i] > d[v][t] + (i - t)) {
					d[v][i] = d[v][t] + (i - t);
					que.push(PI(P(d[v][i], i), v) );
				}
			}
		}

		rep(i, G[v].size()) {
			edge e = G[v][i];
			int nt = t - e.cost;
			if(nt >= 0 && d[e.to][nt] > d[v][t] + e.cost) {
				d[e.to][nt] = d[v][t] + e.cost;
				que.push(PI(P(d[e.to][nt], nt), e.to));
			}
		}
	}
}


int main() {
	while(cin >> n >> m >> l >> k >> a >> h) {
		if(n == 0 && m == 0 && l == 0 && k == 0 && a == 0 && h == 0) break;

		rep(i, 105) G[i].clear();
		memset(L, 0, sizeof(L));

		rep(i, l) {
			int x;
			cin >> x;
			L[x] = true;
		}

		rep(i, k) {
			int s, t, c;
			cin >> s >> t >> c;
			G[s].push_back(edge(t, c));
			G[t].push_back(edge(s, c));
		}

		dijkstra(a);

		int ans = INF;
		rep(i, m + 1) {
			ans = min(ans, d[h][i]);
		}

		if(ans == INF) cout << "Help!" << endl;
		else cout << ans << endl;
	}
	return 0;
}
```

