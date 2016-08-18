---
layout: post
title: "AOJ2583 JAG-channel"
date: 2016-03-21 23:32:18 +0900
comments: true
categories: [AOJ-ICPC, 250]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-type="article" data-card-branding="0"><h4><a href="http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=2583">JAG-channel | Aizu Online Judge</a></h4><p>Introduction to Programming Introduction to Algorithms and Data Structures Library of Data Structures Library of Graph Algorithms Library of Computational Geometry Library of Dynamic Programming Library of Number Theory</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

左の点を {% m %} + {% em %}に変えて，他を空白に変える． {% m %} +{% em %}に変えた場所の上を見ていき，空白ならば {% m %} | {% em %}に変える．

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

int n;
vector<string> v;

int main() {
	while(cin >> n) {
		v.resize(n);
		rep(i, n) cin >> v[i];

		rep(i, n) {
			bool flag = true;
			for(int j = v[i].size()-1; j >= 0; j--) {
				if(v[i][j] == '.') {
					if(flag) {
						v[i][j] = '+';
						flag = false;
					} else {
						v[i][j] = ' ';
					}
				}
			}
		}

		for(int i = n-1; i >= 0; i--) {
			for(int j = v[i].size()-1; j >= 0; j--) {
				if(v[i][j] == '+') {
					int k = i;
					while(k-1 >= 0 && v[k-1][j] == ' ') {
						v[k-1][j] = '|';
						k--;
					}
				}
			}
		}

		rep(i, n) {
			cout << v[i] << endl;
		}
	}
	return 0;
}
```

