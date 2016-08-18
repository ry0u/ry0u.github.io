---
layout: post
title: "AOJ0569 Illumination"
date: 2016-02-16 22:49:54 +0900
comments: true
categories: [AOJ,六角座標]
---

問題文  
http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=0569

<!-- more -->

六角座標の問題．yが偶奇でdy, dxを変えれば良い．  
全てがイルミネーションの場合もあるので，まず全体を1マス余分にとる．  
外の部分は，必ず{% m %} (0, 0) から0で繋がっているはずなので，探索中に回りにある1の数を{% em %}数えれたらACした．

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

int w, h;
int dy[6] = {-1, 0,+1,+1, 0,-1};
int dx[6] = { 0,-1, 0,+1,+1,+1};

int dy2[6] = {-1, 0,+1,+1, 0,-1};
int dx2[6] = {-1,-1,-1, 0,+1, 0};

bool can(int y,int x) {
	if(0 <= y && y < h + 2 && 0 <= x && x < w + 2) return true;
	return false;
}

int main() {
	cin >> w >> h;

	vector< vector<int> > v(h + 2, vector<int>(w + 2));
	rep(i, h) {
		rep(j, w) cin >> v[i+1][j+1];
	}

	int sy = 0, sx = 0;

	queue<P> que;
	que.push(mp(sy, sx));

	bool used[105][105];
	memset(used, 0, sizeof(used));
	used[sy][sx] = true;

	int ans = 0;

	while(que.size()) {
		P p = que.front();
		que.pop();

		rep(i, 6) {
			int y, x;

			if(p.first % 2 == 0) {
				y = p.first + dy2[i];
				x = p.second + dx2[i];
			} else {
				y = p.first + dy[i];
				x = p.second + dx[i];
			}

			if(can(y, x)) {
				if(v[y][x] == 1) {
					ans++;
				} else if(!used[y][x]) {
					used[y][x] = true;
					que.push(mp(y, x));
				}
			}
		}
	}

	cout << ans << endl;

	return 0;
}

```

