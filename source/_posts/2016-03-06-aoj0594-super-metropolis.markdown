---
layout: post
title: "AOJ0594 Super Metropolis"
date: 2016-03-06 01:03:49 +0900
comments: true
categories: [AOJ]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-branding="0"><h4><a href="http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=0594">Super Metropolis | Aizu Online Judge</a></h4><p>Introduction to Programming Introduction to Algorithms and Data Structures Library of Data Structures Library of Graph Algorithms Library of Computational Geometry Library of Dynamic Programming Library of Number Theory</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

右上または左下に行く場合は，斜めの移動が使えるので{% m %} \max(dx, dy) {% em %}でいける．右下または左上に行く場合は{% m %} dx + dy {% em %}かかる．

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
	int w, h, n;
	cin >> w >> h >> n;

	int x, y;
	cin >> x >> y;

	int ans = 0;
	rep(i, n - 1) {
		int a, b;
		cin >> a >> b;

		if(x <= a) {
			if(y <= b) {
				int c = a - x, d = b - y;
				ans += max(c, d);
			} else {
				int c = a - x, d = y - b;
				ans += c + d;
			}
		} else {
			if(y <= b) {
				int c = x - a, d = b - y;
				ans += c + d;
			} else {
				int c = x - a, d = y - b;
				ans += max(c, d);
			}
		}

		x = a;
		y = b;
	}

	cout << ans << endl;

	return 0;
}
```
