---
layout: post
title: "AOJ2153 Mirror Cave"
date: 2016-05-21 02:18:25 +0900
comments: true
categories: [AOJ-ICPC, 350, 幅優先探索]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-branding="0" data-card-type="article"><h4><a href="http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=2153">Mirror Cave | Aizu Online Judge</a></h4><p>Introduction to Programming Introduction to Algorithms and Data Structures Library of Data Structures Library of Graph Algorithms Library of Computational Geometry Library of Dynamic Programming Library of Number Theory</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

状態を{% m %}(ay, ax, by, bx){% em %}として幅優先探索．このようにすれば，片方が移動して片方が壁で立ち止まる場合も表現出来る．左右を逆にするのが面倒だったので，まず最初にLenの方の部屋を反転して，同じ {% m %} (dy, dx) {% em %}を使った．


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

int h, w;
int sax, say, sbx, sby, gax, gay, gbx, gby;
int dx[4] = {1,0,-1,0};
int dy[4] = {0,1,0,-1};
bool used[55][55][55][55];

bool can(int y, int x) {
	if(0 <= y && y < h && 0 <= x && x < w) return true;
	return false;
}

int main() {
	while(cin >> w >> h) {
		if(w == 0 && h == 0) break;

		string A[h], B[h];
		rep(i, h) cin >> A[i] >> B[i];
		rep(i, h) reverse(B[i].begin(), B[i].end());

		rep(i, h) {
			rep(j, w) {
				if(A[i][j] == 'L') {
					say = i;
					sax = j;
					A[i][j] = '.';
				}

				if(B[i][j] == 'R') {
					sby = i;
					sbx = j;
					B[i][j] = '.';
				}

				if(A[i][j] == '%') {
					gay = i;
					gax = j;
					A[i][j] = '.';
				}

				if(B[i][j] == '%') {
					gby = i;
					gbx = j;
					B[i][j] = '.';
				}
			}
		}

		queue<pair< P, P > > que;
		que.push(mp( mp(say, sax), mp(sby, sbx) ));

		memset(used, 0, sizeof(used));
		used[say][sax][sby][sbx] = true;

		bool flag = false;

		while(que.size()) {
			pair<P, P> p = que.front(); que.pop();

			int ay = p.first.first, ax = p.first.second;
			int by = p.second.first, bx = p.second.second;

			if(ay == gay && ax == gax && by == gby && bx == gbx) {
				flag = true;
				break;
			}

			if((ay == gay && ax == gax) || (by == gby && bx == gbx)) {
				continue;
			}

			rep(i, 4) {
				int nay = ay + dy[i];
				int nax = ax + dx[i];

				int nby = by + dy[i];
				int nbx = bx + dx[i];

				if(!can(nay, nax) || A[nay][nax] == '#') {
					nay -= dy[i];
					nax -= dx[i];
				}

				if(!can(nby, nbx) || B[nby][nbx] == '#') {
					nby -= dy[i];
					nbx -= dx[i];
				}

				if(!used[nay][nax][nby][nbx]) {
					que.push(mp( mp(nay, nax), mp(nby, nbx) ));
					used[nay][nax][nby][nbx] = true;
				}
			}
		}

		if(flag) {
			cout << "Yes" << endl;
		} else {
			cout << "No" << endl;
		}
	}
	return 0;
}
```

