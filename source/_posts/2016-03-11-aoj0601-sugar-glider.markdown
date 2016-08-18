---
layout: post
title: "AOJ0601 Sugar Glider"
date: 2016-03-11 16:43:50 +0900
comments: true
categories: [AOJ, dijkstra]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-type="article" data-card-branding="0"><h4><a href="http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=0601">Sugar Glider | Aizu Online Judge</a></h4><p>Introduction to Programming Introduction to Algorithms and Data Structures Library of Data Structures Library of Graph Algorithms Library of Computational Geometry Library of Dynamic Programming Library of Number Theory</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

現在の場所までのコスト，高さを持ってdijkstra．次の木に飛び移る時に高さが{% m %} 0 {% em %}より小さくなる場合，不足分を登ってから飛び移る．この時に，現在の木の高さが(現在の高さ+不足分)より小さいかをチェックする．また，次の木の高さを超えて場合，逆に下ってから飛び移る．この時に，不足分下った高さが{% m %} 0 {% em %}より大きいかどうかをチェックする．  
最終目標が木{% m %} N {% em %}の一番上なので，答えは{% m %} d\[n-1\] + (H\[n-1\] - Y\[n-1\]) {% em %}となる．

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
#define INF 1LL<<62
#define pb push_back
#define mp make_pair

using namespace std;
typedef long long ll;
typedef pair<ll, ll> P;
typedef pair<P, ll> PP;

struct edge {
	int from,to;
	ll cost;

	edge(int t,int c) : to(t),cost(c) {}
	edge(int f,int t,int c) : from(f),to(t),cost(c) {}

	bool operator<(const edge &e) const {
		return cost < e.cost;
	}
};

ll H[100005], Y[100005];
vector<edge> G[100005];
ll d[100005];

void dijkstra(int s, int n, int h) {
	priority_queue<PP,vector<PP>,greater<PP> > que;
	fill(d,d+n,INF);

	d[s] = 0;
	Y[s] = h;
	que.push(PP(P(0, h), s));

	while(que.size()) {
		PP p = que.top();
		que.pop();

		ll cost = p.first.first;
		ll cur = p.first.second;
		int v = p.second;
		if(d[v] < cost) continue;

		rep(i,G[v].size()) {
			edge e = G[v][i];

			if(H[v] < e.cost) continue;

			if(cur - e.cost < 0) {
				if(e.cost - cur <= H[v] - cur && d[e.to] > d[v] + (e.cost - cur) + e.cost) {
					d[e.to] = d[v] + (e.cost - cur) + e.cost;
					Y[e.to] = 0;
					que.push(PP(P(d[e.to], 0), e.to));
				}
			} else if(cur - e.cost > H[e.to]) {
				if(cur - ((cur - e.cost) - H[e.to]) >= 0 && d[e.to] > d[v] + ((cur - e.cost) - H[e.to]) + e.cost) {
					d[e.to] = d[v] + ((cur - e.cost) - H[e.to]) + e.cost;
					Y[e.to] = cur - ((cur - e.cost) - H[e.to]) - e.cost;
					que.push(PP(P(d[e.to], cur - ((cur - e.cost) - H[e.to]) - e.cost), e.to));
				}
			} else {
				if(d[e.to] > d[v] + e.cost) {
					d[e.to] = d[v] + e.cost;
					Y[e.to] = cur - e.cost;
					que.push(PP(P(d[e.to], cur - e.cost), e.to));
				}
			}
		}
	}
}

int main() {
	int n, m, h;
	cin >> n >> m >> h;

	memset(H, 0, sizeof(H));
	memset(Y, 0, sizeof(Y));

	rep(i, n) cin >> H[i];
	rep(i, m) {
		ll a, b, c;
		cin >> a >> b >> c;

		a--;
		b--;

		G[a].push_back(edge(b, c));
		G[b].push_back(edge(a, c));
	}
	
	dijkstra(0, n, h);

	if(d[n-1] == INF) cout << -1 << endl;
	else cout << d[n-1] + (H[n-1] - Y[n-1]) << endl;

	return 0;
}
```
