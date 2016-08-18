---
layout: post
title: "AOJ0590 Available Areas"
date: 2016-03-06 00:49:16 +0900
comments: true
categories: [AOJ]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-type="article" data-card-branding="0"><h4><a href="http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=0590">Available Areas | Aizu Online Judge</a></h4><p>Introduction to Programming Introduction to Algorithms and Data Structures Library of Data Structures Library of Graph Algorithms Library of Computational Geometry Library of Dynamic Programming Library of Number Theory</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

{% math %}
	2xy + x + y
{% endmath %}
で表せるか調べる．{% m %} x {% em %}の値を決めた時に，  キッチンを引いた長方形が存在するかどうかを調べた．

# Code
解説書いてる時に気付いたけど，{% m %} yとx {% em %}が逆だ．

```cpp
#include <iostream>
#include <vector>
#include <string>
#include <cstring>
#include <algorithm>
#include <sstream>
#include <map>
#include <set>
#include <cmath>

#define REP(i,k,n) for(int i=k;i<n;i++)
#define rep(i,n) for(int i=0;i<n;i++)
#define INF 1<<30
#define pb push_back
#define mp make_pair

using namespace std;
typedef long long ll;
typedef pair<int,int> P;

int main() {
	int n;
	cin >> n;

	int ans = 0;
	rep(i, n) {
		int a;
		cin >> a;

		bool flag = false;
		for(int y = 1; 2 * y * y + 2 * y <= a; y++) {
			if((a - y) % (2 * y + 1) == 0) {
				flag = true;
			}
		}

		if(flag) ans++;
	}

	cout << n - ans << endl;

	return 0;
}
```
