---
layout: post
title: "ABC012D バスと割けられない運命"
date: 2016-04-03 17:27:51 +0900
comments: true
categories: [ABC, AtCoder, グラフ, dijkstra]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-branding="0" data-card-type="article-full"><h4><a href="http://abc012.contest.atcoder.jp/tasks/abc012_4">D: バスと避けられない運命 - AtCoder Beginner Contest 012 | AtCoder</a></h4><p>(null)</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

バス停の数{% m %} N {% em %}が {% m %} 1 \leq N \leq 300 {% em %}と小さいので，愚直にそこを始点とした最短経路を求め，最大値の最小値を取った． {% m %} O(N ^2) {% em %}．

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
#define INF 1LL<<60
#define pb push_back
#define mp make_pair

using namespace std;
typedef long long ll;
typedef pair<int,int> P;

struct edge {
	int from,to;
	ll cost;

	edge(int t, ll c) : to(t),cost(c) {}
	edge(int f, int t, ll c) : from(f),to(t),cost(c) {}

	bool operator<(const edge &e) const {
		return cost < e.cost;
	}
};

vector<edge> G[305];
ll d[305];

void dijkstra(int s, int n) {
	priority_queue<P, vector<P>, greater<P> > que;
	fill(d, d+n, INF);

	d[s] = 0;
	que.push(P(0,s));

	while(que.size()) {
		P p = que.top();
		que.pop();

		int v = p.second;
		if(d[v] < p.first) continue;

		rep(i, G[v].size()) {
			edge e = G[v][i];
			if(d[e.to] > d[v] + e.cost) {
				d[e.to] = d[v] + e.cost;
				que.push(P(d[e.to],e.to));
			}
		}
	}
}


int main() {
	int n, m;
	cin >> n >> m;

	rep(i, m) {
		ll s, t, c;
		cin >> s >> t >> c;
		s--; t--;

		G[s].push_back(edge(t, c));
		G[t].push_back(edge(s, c));
	}

	ll ans = INF;
	rep(i, n) {
		dijkstra(i, n);
		ll res = 0;
		rep(j, n) {
			res = max(res, d[j]);
		}

		ans = min(ans, res);
	}

	cout << ans << endl;

	return 0;
}
```

