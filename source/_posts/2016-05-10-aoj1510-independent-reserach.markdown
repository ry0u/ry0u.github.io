---
layout: post
title: "AOJ1510 Independent Reserach"
date: 2016-05-10 00:43:06 +0900
comments: true
categories: [AOJ, Vol15, 実装]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-branding="0" data-card-type="article"><h4><a href="http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=1510">Independent Research | Aizu Online Judge</a></h4><p>Introduction to Programming Introduction to Algorithms and Data Structures Library of Data Structures Library of Graph Algorithms Library of Computational Geometry Library of Dynamic Programming Library of Number Theory</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->
グリッドが {% m %} 5 \times 5 \times 5 {% em %}， {% m %} N \leq 100 {% em %}なので，愚直にシュミレーション．そのマスの周り {% m %} 26 {% em %}マスに生息している生物の数を数えて，そのマスの誕生と死滅を判断する．  

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

int n, c = 0;
int v[5][5][5], v2[5][5][5];
bool a[30], b[30];

int dx[26] = { 1,-1,	 0, 0,	   0, 0,	 1, 1,-1,-1,	 1, 1,-1,-1,	 0, 0, 0, 0,	 1, 1, 1, 1,	-1,-1,-1,-1};
int dy[26] = { 0, 0,	 1,-1,	   0, 0,	 1,-1, 1,-1,	 0, 0, 0, 0,	 1, 1,-1,-1,	 1, 1,-1,-1,	 1, 1,-1,-1};
int dz[26] = { 0, 0,	 0, 0,	   1,-1,	 0, 0, 0, 0,	 1,-1, 1,-1,	 1,-1, 1,-1,	 1,-1, 1,-1,	 1,-1, 1,-1};

bool can(int x,int y,int z) {
	if(0 <= x && x < 5 && 0 <= y && y < 5 && 0 <= z && z < 5) return true;
	return false;
}

int main() {
	while(cin >> n && n) {
		memset(v, 0, sizeof(v));
		memset(v2, 0, sizeof(v2));

		rep(i, 5) {
			rep(j, 5) {
				string s;
				cin >> s;

				rep(k, 5) {
					v[i][j][k] = s[k] - '0';
				}
			}
		}

		int m1;
		cin >> m1;

		memset(a, 0, sizeof(a));
		rep(i, m1) {
			int x;
			cin >> x;
			a[x] = true;
		}

		int m2;
		cin >> m2;
		memset(b, 0, sizeof(b));
		rep(i, m2) {
			int x;
			cin >> x;
			b[x] = true;
		}

		rep(q, n) {
			rep(i, 5) rep(j, 5) rep(k, 5) v2[i][j][k] = v[i][j][k];
			rep(i, 5) {
				rep(j, 5) {
					rep(k, 5) {
						int sum = 0;

						rep(l, 26) {
							int x = i + dx[l];
							int y = j + dy[l];
							int z = k + dz[l];

							if(can(x, y, z)) sum += v[x][y][z];
						}

						if(v[i][j][k] == 0) {
							if(a[sum]) {
								v2[i][j][k] = 1;
							}
						} else {
							if(!b[sum]) {
								v2[i][j][k] = 0;
							}
						}
					}
				}
			}

			rep(i, 5) rep(j, 5) rep(k, 5) v[i][j][k] = v2[i][j][k];
		}

		if(c) cout << endl;
		cout << "Case " << c + 1 << ":" << endl;
		c++;

		rep(i, 5) {
			if(i) cout << endl;
			rep(j, 5) {
				rep(k, 5) {
					cout << v[i][j][k];
				}
				cout << endl;
			}
		}
	}

	return 0;
}
```

