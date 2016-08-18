---
layout: post
title: "AOJ2299 Tiles are Colorful"
date: 2016-05-28 23:31:22 +0900
comments: true
categories: [AOJ-ICPC, 350]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-branding="0" data-card-type="article-full"><h4><a href="http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=2299">Tiles are Colorful | Aizu Online Judge</a></h4><p>Introduction to Programming Introduction to Algorithms and Data Structures Library of Data Structures Library of Graph Algorithms Library of Computational Geometry Library of Dynamic Programming Library of Number Theory</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

マスを叩くとその，上下左右各方向に進んでいき，最初に当たったタイルに同じ色があればそのタイルを消す．最大で何個消せるか？

> 1 ≤ M ≤ 500，1 ≤ N ≤ 500 を満たす．各アルファベット大文字は入力中に 0 個または 2 個現れる．

Inputの一番最後に重要なことが書いてあって最初は見逃していた．つまり各アルファベットの対が {% m %} 1 {% em %}個あるかないか，という制約である．各ペアのアルファベットを消すために，どのアルファベットが消されている必要があるかを考える．特に依存関係が無ければそのタイルを消し，他のアルファベットの依存関係になっている箇所があればそこも消す．これを繰り返す．  
縦，横，斜めのパターンに分けて依存関係を調べた．自分の実装では斜めのパターンが結構面倒だった．

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
	int m, n;
	cin >> m >> n;

	vector<string> v(m);
	rep(i, m) cin >> v[i];

	map<char, vector<int> > x, y;
	rep(i, m) {
		rep(j, n) {
			if('A' <= v[i][j] && v[i][j] <= 'Z') {
				y[v[i][j]].push_back(i);
				x[v[i][j]].push_back(j);
			}
		}
	}

	set<char> st[30][2];
	bool used[30][2];
	memset(used, 0, sizeof(used));

	rep(i, 26) {
		char c = 'A' + i;
		if(x[c].size() == 0) continue;

		if(x[c][0] == x[c][1]) {
			if(abs(y[c][0] - y[c][1]) == 1) continue;
			used[i][0] = true;
			int x1 = x[c][0];
			int y1 = y[c][0], y2 = y[c][1];

			REP(j, min(y1, y2) + 1, max(y1, y2)) {
				if(v[j][x1] == '.') continue;
				st[i][0].insert(v[j][x1]);
			}
		} else if(y[c][0] == y[c][1]) {
			if(abs(x[c][0] - x[c][1]) == 1) continue;
			used[i][0] = true;
			int y1 = y[c][0];
			int x1 = x[c][0], x2 = x[c][1];
			REP(j, min(x1, x2) + 1, max(x1, x2)) {
				if(v[y1][j] == '.') continue;
				st[i][0].insert(v[y1][j]);
			}
		} else {
			used[i][0] = true;
			used[i][1] = true;
			int x1 = x[c][0], x2 = x[c][1];
			int y1 = y[c][0], y2 = y[c][1];

			REP(j, min(x1, x2), max(x1, x2) + 1) {
				if(v[y1][j] == '.' || v[y1][j] == c) continue;
				st[i][0].insert(v[y1][j]);
			}

			REP(j, min(y1, y2), max(y1, y2) + 1) {
				if(v[j][x2] == '.' || v[j][x2] == c) continue;
				st[i][0].insert(v[j][x2]);
			}

			REP(j, min(y1, y2), max(y1, y2) + 1) {
				if(v[j][x1] == '.' || v[j][x1] == c) continue;
				st[i][1].insert(v[j][x1]);
			}

			REP(j, min(x1, x2), max(x1, x2) + 1) {
				if(v[y2][j] == '.' || v[y2][j] == c) continue;
				st[i][1].insert(v[y2][j]);
			}
		}
	}

	int sum = 0;
	while(true) {
		bool flag = true;
		rep(i, 26) {
			char c = 'A' + i;

			rep(j, 2) {
				if(!used[i][j]) continue;
				if(st[i][j].size() == 0) {
					flag = false;
					used[i][j] = false;
					used[i][!j] = false;

					rep(k, 26) {
						rep(l, 2) {
							st[k][l].erase(c);
						}
					}
					sum += 2;
				}
			}
		}

		if(flag) break;
	}

	cout << sum << endl;


	return 0;
}
```
