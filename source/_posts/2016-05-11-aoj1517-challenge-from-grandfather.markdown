---
layout: post
title: "AOJ1517 Challenge from Grandfather"
date: 2016-05-11 11:35:39 +0900
comments: true
categories: [AOJ, Vol15, 実装]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-branding="0" data-card-type="article"><h4><a href="http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=1517">Challenge from Grandfather | Aizu Online Judge</a></h4><p>Introduction to Programming Introduction to Algorithms and Data Structures Library of Data Structures Library of Graph Algorithms Library of Computational Geometry Library of Dynamic Programming Library of Number Theory</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

部分行列の回転，反転，左シフト，右シフト，島反転を実装する．  
回転は90度回転を{% m %} \frac{angle}{90} {% em %}回行う．  
反転，左シフト，右シフトはその通りにあって，島反転は幅優先で島を発見し反転していった．

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

int n, m;
int t[15][15];
vector<int> v[15];

void vrotate(int r, int c, int size, int angle) {
	if(angle == 0 || angle == 360) return;

	memset(t, 0, sizeof(t));

	rep(i, angle/90) {
		int col = c;
		for(int i = r + size - 1; i >= r; i--, col++) {
			REP(j, c, c + size) {
				t[r+j-c][col] = v[i][j];
			}
		}

		REP(i, r, r + size) {
			REP(j, c, c + size) {
				v[i][j] = t[i][j];
			}
		}
	}
}

void reversal(int r, int c, int size) {
	REP(i, r, min(n, r + size)) {
		REP(j, c, min(n, c + size)) {
			v[i][j] = !v[i][j];
		}
	}
}

void leftshift(int r) {
	int t = v[r][0];
	rep(i, n) {
		v[r][i] = v[r][i+1];
	}
	v[r][n-1] = t;
}

void rightshift(int r) {
	int t = v[r][n-1];
	for(int i = n-1; i >= 1; i--) {
		v[r][i] = v[r][i-1];
	}
	v[r][0] = t;
}

int dx[4] = {1,0,-1,0};
int dy[4] = {0,1,0,-1};
bool used[15][15];

bool can(int y, int x) {
	if(0 <= y && y < n && 0 <= x && x < n) return true;
	return false;
}

void islandreversal(int r, int c) {
	queue<P> que;
	que.push(mp(r, c));

	memset(used, 0, sizeof(used));
	used[r][c] = true;
	int t = v[r][c];
	while(que.size()) {
		P p = que.front(); que.pop();
		int y = p.first;
		int x = p.second;

		if(v[y][x] == t) {
			v[y][x] = !v[y][x];
		}

		rep(i, 4) {
			int ny = y + dy[i];
			int nx = x + dx[i];

			if(can(ny, nx) && !used[ny][nx] && v[ny][nx] == t) {
				que.push(mp(ny, nx));
				used[ny][nx] = true;
			}
		}
	}
}

int main() {
	cin >> n >> m;

	memset(v, 0, sizeof(v));
	rep(i, n) {
		v[i].resize(n);
		rep(j, n) cin >> v[i][j];
	}

	rep(i, m) {
		int o;
		cin >> o;

		int r, c, size, angle;

		if(o == 0) { // rotate
			cin >> r >> c >> size >> angle;
			r--; c--;

			vrotate(r, c, size, angle);
		} else if(o == 1) { // reversal
			cin >> r >> c >> size;
			r--; c--;

			reversal(r, c, size);
		} else if(o == 2) { // left shift
			cin >> r;
			r--;

			leftshift(r);
		} else if(o == 3) { // right shift
			cin >> r;
			r--;
			rightshift(r);
		} else if(o == 4) { // Island reversal
			cin >> r >> c;
			r--; c--;
			islandreversal(r, c);
		}
	}

	rep(i, n) {
		rep(j, n) {
			cout << v[i][j];

			if(j == n-1) cout << endl;
			else cout << " ";
		}
	}

	return 0;
}
```

