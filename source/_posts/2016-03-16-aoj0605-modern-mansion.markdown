---
layout: post
title: "AOJ0605 Modern Mansion"
date: 2016-03-16 01:28:12 +0900
comments: true
categories: [AOJ, グラフ, dijkstra]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-type="article" data-card-branding="0"><h4><a href="http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=0605">Modern Mansion | Aizu Online Judge</a></h4><p>Introduction to Programming Introduction to Algorithms and Data Structures Library of Data Structures Library of Graph Algorithms Library of Computational Geometry Library of Dynamic Programming Library of Number Theory</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

状態を {% m %} (cost, id, state) {% em %}として，現在どちら方向の扉が空いているかを持ってdijkstra．扉が空いている方向で最小値を更新出来る場所に行く．TLEを連発した．時間制限が1secで0.94secでギリギリ通した．変にlogをつけて死んでいた．

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
typedef pair<P, bool> PI;

int w, h, k;
vector<P> X[200005], Y[200005];
vector<P> xy;
ll d[200005][2];

// PI f(int a, int b, int c) {
// 	return mp(mp(a, b), c);
// }

void dijkstra() {
	rep(i, 200005) {
		d[i][0] = INF;
		d[i][1] = INF;
	}

	// cost id (0 or 1)
	priority_queue<PI, vector<PI>, greater<PI> > que;

	rep(i, X[1].size()) {
		int y = X[1][i].first;
		int to = X[1][i].second;

		d[to][1] = y;
		// que.push(f(y, to, 1));
		que.push(mp(mp(y, to), 1));
	}

	while(que.size()) {
		PI p = que.top(); 
		que.pop();

		int c = p.first.first;
		int v = p.first.second;
		int state = p.second;

		if(d[v][state] < c) continue;

		int y = xy[v].first;
		int x = xy[v].second;

		if(state == 0) {
			rep(i, X[x].size()) {
				int ny = X[x][i].first;
				int to = X[x][i].second;

				if(ny == y) continue;

				if(d[to][1] > d[v][state] + abs(ny - y) + 1) {
					d[to][1] = d[v][state] + abs(ny- y) + 1;
					// que.push(f(d[to][1], to, 1));
					que.push(mp(mp(d[to][1], to), 1));
				}
			}
		} else {
			rep(i, Y[y].size()) {
				int nx = Y[y][i].first;
				int to = Y[y][i].second;

				if(nx == x) continue;

				if(d[to][0] > d[v][state] + abs(nx - x) + 1) {
					d[to][0] = d[v][state] + abs(nx - x) + 1;
					// que.push(f(d[to][0], to, 0));
					que.push(mp(mp(d[to][0], to), 0));
				}
			}
		}
	}
}

int main() {
	cin >> w >> h >> k;

	xy.resize(k);
	rep(i, k) {
		int x, y;
		cin >> x >> y;

		xy[i] = mp(y, x);
		X[x].push_back(mp(y, i));
		Y[y].push_back(mp(x, i));
	}

	dijkstra();

	ll ans = INF;
	rep(i, k) {
		int y = xy[i].first;
		int x = xy[i].second;

		if(y == h) {
			ans = min(ans, d[i][1] + abs(w - x));
		}

		if(x == w) {
			ans = min(ans, d[i][0] + abs(h - y));
		}
	}

	if(ans == INF) cout << -1 << endl;
	else cout << ans << endl;

	return 0;
}
```
