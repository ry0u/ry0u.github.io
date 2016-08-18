---
layout: post
title: "AOJ1325 Bit String Recordering"
date: 2016-03-22 20:46:04 +0900
comments: true
categories: [AOJ-ICPC, 250]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-type="article" data-card-branding="0"><h4><a href="http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=1345">Bit String Reordering | Aizu Online Judge</a></h4><p>Introduction to Programming Introduction to Algorithms and Data Structures Library of Data Structures Library of Graph Algorithms Library of Computational Geometry Library of Dynamic Programming Library of Number Theory</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

連続するビット列の数が与えられる．最初を {% m %} 0 {% em %}にする， {% m %} 1 {% em %}にするの {% m %} 2 {% em %}パターンをシュミレーション(左端から決めていき，違ったら右から持ってくる)して，swap回数の小さい方を出力した．

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
	int n, m;
	cin >> n >> m;

	vector<int> a(n), b(m);
	rep(i, n) cin >> a[i];
	rep(i, m) cin >> b[i];


	vector<int> c, d;
	rep(i, m) {
		rep(j, b[i]) {
			if(i & 1) {
				c.push_back(1);
				d.push_back(0);
			} else {
				c.push_back(0);
				d.push_back(1);
			}
		}
	}

	int ans = INF, res = 0;
	vector<int> t(a.begin(), a.end());
	rep(i, n) {
		if(t[i] == c[i]) continue;

		REP(j, i+1, n) {
			if(c[i] == t[j]) {
				for(int k = j; k-1 >= i; k--) {
					swap(t[k], t[k-1]);
					res++;
				}
				break;
			}
		}
	}

	bool flag = true;
	rep(i, n) {
		if(t[i] == c[i]) continue;
		flag = false;
	}

	if(flag) {
		ans = min(ans, res);
	}

	res = 0;
	rep(i, n) t[i] = a[i];

	rep(i, n) {
		if(t[i] == d[i]) continue;

		REP(j, i+1, n) {
			if(d[i] == t[j]) {
				for(int k = j; k-1 >= i; k--) {
					swap(t[k], t[k-1]);
					res++;
				}
				break;
			}

		}
	}

	flag = true;
	rep(i, n) {
		if(t[i] == d[i]) continue;
		flag = false;
	}

	if(flag) {
		ans = min(ans, res);
	}

	cout << ans << endl;

	return 0;
}
```

