---
layout: post
title: "AOJ2241 Usaneko Matrix"
date: 2016-06-04 17:34:11 +0900
comments: true
categories: [AOJ-ICPC, 350]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-branding="0" data-card-type="article-full"><h4><a href="http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=2241">Usaneko Matrix | Aizu Online Judge</a></h4><p>Introduction to Programming Introduction to Algorithms and Data Structures Library of Data Structures Library of Graph Algorithms Library of Computational Geometry Library of Dynamic Programming Library of Number Theory</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

ビンゴゲームをやる．行，列，斜めで揃っているものが$u$，$v$以上になれば勝ちとなる．それぞれの数字がどこの座標にあったかをチェックし，引いた数の座標から，縦横斜めの該当する所を$+1$する．先にどちらかが勝った場合は後は続けなくて良いので，毎回引くごとに合計を確認した．$n=1$は，場合分けした．$O(nm)$


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

int main() {
	int n, u, v, m;
	cin >> n >> u >> v >> m;

	map<int, vector<P> > ma[2];
	vector<vector<int> > a(n, vector<int>(n)), b(n, vector<int>(n));

	rep(i, n) {
		rep(j, n) {
			cin >> a[i][j];
			ma[0][a[i][j]].push_back(mp(i, j));
		}
	}

	rep(i, n) {
		rep(j, n) {
			cin >> b[i][j];
			ma[1][b[i][j]].push_back(mp(i, j));
		}
	}

	vector<int> x(m);
	rep(i, m) cin >> x[i];

	if(n == 1) {
		rep(i, m) {
			int sum[2];
			memset(sum, 0, sizeof(sum));

			rep(j, 2) {
				vector<P> res = ma[j][x[i]];
				if(res.size() >= 1) sum[j]++;
			}

			bool f1 = (sum[0] >= u);
			bool f2 = (sum[1] >= v);

			if(f1 && f2) {
				cout << "DRAW" << endl;
				return 0;
			} else if(f1) {
				cout << "USAGI" << endl;
				return 0;
			} else if(f2) {
				cout << "NEKO" << endl;
				return 0;
			}
		}

		cout << "DRAW" << endl;
		return 0;
	}


	int cy[2][505], cx[2][505], cross[2][2];
	memset(cy, 0, sizeof(cy));
	memset(cx, 0, sizeof(cx));
	memset(cross, 0, sizeof(cross));

	rep(i, m) {

		rep(j, 2) {
			vector<P> res = ma[j][x[i]];
			rep(k, res.size()) {
				int f = res[k].first, s = res[k].second;
				cy[j][f]++;
				cx[j][s]++;

				if(f == s) cross[j][0]++;
				if(f == n - 1 - s) cross[j][1]++;
			}
		}

		int sum[2];
		memset(sum, 0, sizeof(sum));

		rep(j, 2) {
			rep(k, 2) {
				sum[j] += (cross[j][k] == n);
			}
		}

		rep(j, 2) {
			rep(k, n) {
				sum[j] += (cy[j][k] == n);
				sum[j] += (cx[j][k] == n);
			}
		}

		bool f1 = (sum[0] >= u), f2 = (sum[1] >= v);
		if(f1 && f2) {
			cout << "DRAW" << endl;
			return 0;
		} else if(f1) {
			cout << "USAGI" << endl;
			return 0;
		} else if(f2) {
			cout << "NEKO" << endl;
			return 0;
		}
	}

	cout << "DRAW" << endl;

	return 0;
}
```

