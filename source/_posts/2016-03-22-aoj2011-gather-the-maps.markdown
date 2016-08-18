---
layout: post
title: "AOJ2011 Gather the Maps!"
date: 2016-03-22 22:32:45 +0900
comments: true
categories: [AOJ-ICPC, 300]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-type="article" data-card-branding="0"><h4><a href="http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=2011">Gather the Maps! | Aizu Online Judge</a></h4><p>Introduction to Programming Introduction to Algorithms and Data Structures Library of Data Structures Library of Graph Algorithms Library of Computational Geometry Library of Dynamic Programming Library of Number Theory</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

この追記の部分の通り， {% m %} 1日目 {% em %}から順番に誰と会えるかを {% m %} S[i] {% em %}で管理して，初めて {% m %} N {% em %}人揃った時の日にちを出力した． {% m %} N \leq 50 {% em %}なのでllに収まるのでビットで管理した．会う，というのが論理和で出来るので楽に感じた．

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
#include <bitset>

#define REP(i,k,n) for(int i=k;i<n;i++)
#define rep(i,n) for(int i=0;i<n;i++)
#define INF 1<<30
#define pb push_back
#define mp make_pair

using namespace std;
typedef long long ll;
typedef pair<int,int> P;

bool used[55][35];

int main() {
	int n;
	while(cin >> n && n) {

		memset(used, 0, sizeof(used));
		rep(i, n) {
			int x;
			cin >> x;
			rep(j, x) {
				int y;
				cin >> y;

				used[i][y] = true;
			}
		}

		ll S[55];
		memset(S, 0, sizeof(S));

		bool flag = true;
		rep(i, 35) {
			rep(j, n) {
				rep(k, n) {
					if(used[j][i] && used[k][i]) {
						ll sum = (S[j] | S[k]);
						sum |= (1LL << j);
						sum |= (1LL << k);

						S[j] = S[k] = sum;
					}
				}
			}

			rep(j, n) {
				if(__builtin_popcountll(S[j]) == n) {
					cout << i << endl;
					flag = false;
					break;
				}
			}

			if(!flag) break;
		}

		if(flag) cout << -1 << endl;
	}

	return 0;
}
```

