---
layout: post
title: "AOJ1174 Identically Colored Panels Connection"
date: 2016-04-18 23:00:29 +0900
comments: true
categories: [AOJ-ICPC, 350, 実装]
---

<a class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-branding="0" data-card-type="article" href="http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=1174">Identically Colored Panels Connection</a>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

塗る色を決めて順番に塗っていく． それぞれ  

* {% m %} rec {% em %} : 始点から探索して{% m %} c {% em %}と同じパネルが何色あるか
* {% m %} dfs {% em %} : 始点から探索して{% m %} target {% em %}の色を {% m %} change {% em %}にする．
* {% m %} func {% em %} : {% m %} i {% em %}回塗った状態． 最終的に{% m %} 5 {% em %}回塗った回数を答えるが，最後に {% m %} c {% em %}に塗り替えた場合なので，{% m %} 4 {% em %}回塗った後に {% m %} c {% em %}に塗って {% m %} rec {% em %}を呼ぶ．

>ただし，電極は左上角のパネルに固定されていることとする． 

の文を見逃していて，塗り始める場所を全探索していてSampleがずっと合わなかった...  
全体的に辛い

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

int h, w, c;
int dx[4] = {1,0,-1,0};
int dy[4] = {0,1,0,-1};

bool can(int y,int x) {
	if(0 <= y && y < h && 0 <= x && x < w) return true;
	return false;
}

int cnt = 0, sy = 0, sx = 0;
bool used[10][10];
vector< vector<int> > v;

void rec(int y, int x) {
	used[y][x] = true;
	cnt++;

	rep(i, 4) {
		int ny = y + dy[i];
		int nx = x + dx[i];

		if(can(ny, nx) && !used[ny][nx] && v[ny][nx] == c) {
			rec(ny, nx);
		}
	}
}

int target, change;

void dfs(int y, int x) {
	v[y][x] = change;
	rep(i, 4) {
		int ny = y + dy[i];
		int nx = x + dx[i];

		if(can(ny, nx) && v[ny][nx] == target) {
			dfs(ny, nx);
		}
	}
}

int ans = 0;
void func(vector< vector<int> > t, int id) {
	// cout << " ------ func ---- :" << id << endl;
	// rep(i, h) {
	// 	rep(j, w) cout << t[i][j] << " ";
	// 	cout << endl;
	// }
	if(id == 4) {
		v = t;
		target = t[sy][sx];

		if(target == c) return;
		change = c;
		dfs(sy, sx);

		cnt = 0;
		memset(used, 0, sizeof(used));
		rec(sy, sx);
		ans = max(ans, cnt);
		return;
	}

	REP(i, 1, 7) {
		if(i == t[sy][sx]) continue;
		v = t;
		target = t[sy][sx]; change = i;
		dfs(sy, sx);
		func(v, id + 1);
	}
}

int main() {
	while(cin >> h >> w >> c) {
		if(h == 0 && w == 0 && c == 0) break;

		v.resize(h);
		rep(i, h) {
			v[i].resize(w);
			rep(j, w) cin >> v[i][j];
		}

		ans = 0;
		func(v, 0);

		cout << ans << endl;
	}
	return 0;
}
```

