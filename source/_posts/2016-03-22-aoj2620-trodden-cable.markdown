---
layout: post
title: "AOJ2620 Trodden Cable"
date: 2016-03-22 21:52:44 +0900
comments: true
categories: [AOJ-ICPC, 250, グラフ, dijkstra]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-type="article" data-card-branding="0"><h4><a href="http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=2620">Trodden Cable</a></h4><p>Nathan O. Davis is running a company. His company owns a web service which has a lot of users. So his office is full of servers, routers and messy LAN cables. He is now very puzzling over the messy cables, because they are causing many kinds of problems. For example, staff working at the company often trip over a cable.</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

全く分からずに調べた．

http://acm-icpc.aitea.net/index.php?plugin=attach&refer=2013%2FPractice%2F%E5%A4%8F%E5%90%88%E5%AE%BF%2F%E8%AC%9B%E8%A9%95&openfile=trodden_cable.pdf:image=http://acm-icpc.aitea.net/index.php?plugin=attach&refer=2013%2FPractice%2F%E5%A4%8F%E5%90%88%E5%AE%BF%2F%E8%AC%9B%E8%A9%95&openfile=trodden_cable.pdf  

この画像が非常に分かりやすい．移動する時にまたがる線のコストを{% m %} +1 {% em %}して，移動が終わったグリッドグラフでdijkstra．  

現在位置を(y, x)と置くと  

* {% m %} R {% em %}の時， {% m %} (y, x+1) \to (y+1, x+1) {% em %}
* {% m %} L {% em %}の時， {% m %} (y, x) \to (y+1, x) {% em %}
* {% m %} U {% em %}の時， {% m %} (y, x) \to (y, x+1) {% em %}
* {% m %} D {% em %}の時， {% m %} (y+1, x) \to (y+1, x+1) {% em %}

に {% m %} +1 {% em %}した．

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

int w, h, n;
int sx,sy,gx,gy;
int dx[4] = {1,0,-1,0};
int dy[4] = {0,1,0,-1};

bool can(int y, int x) {
	if(0 <= y && y < h && 0 <= x && x < w) return true;
	return false;
}

struct edge {
	int from,to;
	ll cost;

	edge(int t,int c) : to(t),cost(c) {}
	edge(int f,int t,int c) : from(f),to(t),cost(c) {}

	bool operator<(const edge &e) const {
		return cost < e.cost;
	}
};

vector<edge> G[505*505 + 5];
ll d[505 * 505 + 5];

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
	cin >> w >> h >> n;

	cin >> sx >> sy >> gx >> gy;

	rep(i, h * w) {
		G[i].clear();
	}

	w++;
	h++;

	rep(i, h) {
		rep(j, w) {
			rep(k, 4) {
				int y = i + dy[k];
				int x = j + dx[k];

				if(can(y, x)) {
					G[i*w + j].push_back(edge(y*w + x, 0));
				}
			}
		}
	}

	rep(i, n) {
		int y, x, t;
		cin >> x >> y >> t;

		string s;
		cin >> s;

		rep(k, t) {
			rep(j, s.size()) {
				int from = -1, to = -1;
				if(s[j] == 'R' && x + 1 < w - 1) {
					from = y * w + (x + 1);
					to = (y + 1) * w + (x + 1);
					x++;
				} else if(s[j] == 'L' && 0 <= x - 1) {
					from = y * w + x;
					to = (y + 1) * w + x;
					x--;
				} else if(s[j] == 'U' && 0 <= y - 1) {
					from = y * w + x;
					to = y * w + (x + 1);
					y--;
				} else if(s[j] == 'D' && y + 1 < h - 1) {
					from = (y + 1) * w + x;
					to = (y + 1) * w + (x + 1);
					y++;
				}

				if(from == -1 && to == -1) continue;
				rep(k, G[from].size()) {
					if(G[from][k].to == to) {
						G[from][k].cost++;
					}
				}
				rep(k, G[to].size()) {
					if(G[to][k].to == from) {
						G[to][k].cost++;
					}
				}
			}
		}
	}

	// rep(i, h) {
	// 	rep(j, w) {
	// 		cout << " --------- " << i << ", " << j << " :" << i * w + j << " |";
	// 		rep(k, G[i*w + j].size()) {
	// 			cout << "(" << G[i*w + j][k].to << ", " << G[i*w + j][k].cost << ") ";
	// 		}
	// 		cout << endl;
	// 	}
	// }

	dijkstra(sy * w + sx, h * w);

	// rep(i, h) {
	// 	rep(j, w) {
	// 		cout << d[i*w + j] << " ";
	// 	}
	// 	cout << endl;
	// }

	cout << d[gy * w + gx] << endl;

	return 0;
}
```

