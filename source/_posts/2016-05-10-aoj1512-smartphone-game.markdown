---
layout: post
title: "AOJ1512 Smartphone Game"
date: 2016-05-10 00:50:38 +0900
comments: true
categories: [AOJ, Vol15, 実装]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-branding="0" data-card-type="article"><h4><a href="http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=1512">Smartphone Game | Aizu Online Judge</a></h4><p>Introduction to Programming Introduction to Algorithms and Data Structures Library of Data Structures Library of Graph Algorithms Library of Computational Geometry Library of Dynamic Programming Library of Number Theory</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

> ブロックを任意に1つだけ決めて、最大でn回まで上下左右に移動できる。移動先のブロックは移動元のブロックのあった場所に移動する。つまり、隣接したブロックを交換する事になる。

この部分が理解出来ていなかった．普通に {% m %} n {% em %}回まで，隣接しているブロック同士をswapしていて {% m %} 1 {% em %}つのブロックだけをswapしていくことが出来ていなかった．  
求めるのは{% m %} 1 {% em %}回のプレイで得られる最大の点数なので，まずは {% m %} n {% em %}回までswapしたブロックの状態をsetに突っ込む．その後，削除，移動で変化が無くなるまで続ける．

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

int cost[6];
bool used[5][5], flag;
int dx[4] = {1,0,-1,0};
int dy[4] = {0,1,0,-1};

bool can(int y,int x) {
	if(0 <= y && y < 5 && 0 <= x && x < 5) return true;
	return false;
}

ll del(vector<vector<int> > &v, ll bonus) {
	memset(used, 0, sizeof(used));
	rep(i, 5) {
		rep(j, 5) {
			if(v[i][j] == 0) continue;

			int a = i;
			while(a + 1 <= 4 && v[a+1][j] == v[i][j]) {
				a++;
			}

			int b = i;
			while(b - 1 >= 0 && v[b-1][j] == v[i][j]) {
				b--;
			}

			int c = j;
			while(c + 1 <= 4 && v[i][c+1] == v[i][j]) {
				c++;
			}

			int d = j;
			while(d - 1 >= 0 && v[i][d-1] == v[i][j]) {
				d--;
			}

			if(a - b + 1 >= 3 || c - d + 1 >= 3) used[i][j] = true;
		}
	}

	ll ret = 0;
	rep(i, 5) {
		rep(j, 5) {
			if(used[i][j]) {
				ret += bonus * cost[v[i][j]];
				flag = true;
				v[i][j] = 0;
			}
		}
	}

	return ret;
}

void mov(vector<vector<int> > &v) {
	for(int i = 4; i >= 0; i--) {
		rep(j, 5) {
			int y = i;
			while(y + 1 <= 4 && v[y + 1][j] == 0) {
				swap(v[y][j], v[y+1][j]);
				y++;
			}
		}
	}
}

int n;
ll ans = 0;

set<vector<vector<int> > > res;

void dfs(int cnt, int y, int x, vector<vector<int> > v) {
	res.insert(v);

	if(cnt == n) {
		return;
	}

	rep(i, 4) {
		int ny = y + dy[i];
		int nx = x + dx[i];

		if(can(ny, nx)) {
			swap(v[y][x], v[ny][nx]);
			dfs(cnt + 1, ny, nx, v);
			swap(v[y][x], v[ny][nx]);
		}
	}
}

int main() {
	while(cin >> n && n != -1) {
		ans = 0;
		res.clear();

		vector<vector<int> > v(5, vector<int>(5));
		rep(i, 5) rep(j, 5) cin >> v[i][j];
		rep(i, 5) cin >> cost[i+1];

		rep(i, 5) {
			rep(j, 5) {
				dfs(0, i, j, v);
			}
		}

		set<vector<vector<int> > >::iterator ite;
		for(ite = res.begin(); ite != res.end(); ite++) {

			ll bonus = 1, sum = 0;
			vector<vector<int> > t = *ite;

			while(true) {
				flag = false;
				sum += del(t, bonus);

				if(flag) {
					mov(t);
					bonus++;

				} else break;
			}

			// cout << "----- : " << sum << endl;
			// rep(i, 5) {
			// 	rep(j, 5) {
			// 		cout << t[i][j] << " ";
			// 	}
			// 	cout << endl;
			// }
			ans = max(ans, sum);
		}

		cout << ans << endl;
	}

	return 0;
}
```
