---
layout: post
title: "AOJ0584 Two Number Cards"
date: 2016-03-05 23:44:14 +0900
comments: true
categories: [AOJ]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-type="article" data-card-branding="0"><h4><a href="http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=0584">Two Number Cards | Aizu Online Judge</a></h4><p>Introduction to Programming Introduction to Algorithms and Data Structures Library of Data Structures Library of Graph Algorithms Library of Computational Geometry Library of Dynamic Programming Library of Number Theory</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

制約で{% m %} 3 \leq n \leq 4000 {% em %}なので，愚直にpriority_queueとかに突っ込むと{% m %} n ^2 log n ^2 {% em %}で死ぬ．  
3番目まで取っておく．  
また数の連結もstringstreamを用いたらTLEした．

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

#define rep(i,k,n) for(int i=k;i<n;i++)
#define rep(i,n) for(int i=0;i<n;i++)
#define inf 1<<30
#define pb push_back
#define mp make_pair

using namespace std;
typedef long long ll;
typedef pair<int,int> p;

int main() {
	int n;
	cin >> n;

	vector<int> v(n);
	rep(i, n) cin >> v[i];

	sort(v.begin(), v.end());

	int a = inf, b = inf, c = inf, x;
	rep(i, n) {
		rep(j, n) {
			if(i == j) continue;

			int x = v[i];
			for(int k = v[j]; k > 0; k /= 10) {
				x *= 10;
			}

			x += v[j];

			if(x <= a) {
				c = b;
				b = a;
				a = x;
			} else if(x <= b) {
				c = b;
				b = x;
			} else if(x <= c) {
				c = x;
			}
		}
	}

	cout << c << endl;

	return 0;
}
```

