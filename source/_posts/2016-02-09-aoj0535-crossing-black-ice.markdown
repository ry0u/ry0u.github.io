---
layout: post
title: "AOJ0535 Crossing Black Ice"
date: 2016-02-09 14:46:39 +0900
comments: true
categories: [AOJ, dfs]
---

問題文  
http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=0535

<!-- more -->

移動方法は20万通りを超えないと問題分に明記されているので，普通にDFSした．移動できる区画数の最大値を求めたいので移動出来る場所がなくなった時点でのMaxを返すようにした．


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

int w,h;
int dx[4] = {1,0,-1,0};
int dy[4] = {0,1,0,-1};
int f[100][100];
int ans = 0;

bool can(int y,int x) {
	if(0 <= y && y < h && 0 <= x && x < w) return true;
	return false;
}

void dfs(set<int> S, int y, int x) {
	bool flag = true;
	rep(i, 4) {
		int ny = y + dy[i];
		int nx = x + dx[i];
		int id = ny * h + nx;

		if(can(ny, nx) && f[ny][nx] == 1 && S.find(id) == S.end() ) {
			flag = false;
			S.insert(id);
			dfs(S, ny, nx);
			S.erase(id);
		}
	}

	if(flag) {
		ans = max(ans, (int)S.size());
	}
}

int main() {
	while(cin >> w >> h) {
		if(w == 0 && h == 0) break;

		rep(i, h) {
			rep(j, w) cin >> f[i][j];
		}

		ans = 0;
		rep(i, h) {
			rep(j, w) {
				if(f[i][j]){
					set<int> S;
					S.insert(i * h + j);
					dfs(S, i, j);
				}
			}
		}

		cout << ans << endl;
	}
	return 0;
}
```
