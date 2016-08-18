---
layout: post
title: "AOJ1509 Rental DVD Shop NEO"
date: 2016-05-10 00:29:00 +0900
comments: true
categories: [AOJ, Vol15, 貪欲]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-branding="0" data-card-type="article"><h4><a href="http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=1509">Rental DVD Shop NEO | Aizu Online Judge</a></h4><p>Introduction to Programming Introduction to Algorithms and Data Structures Library of Data Structures Library of Graph Algorithms Library of Computational Geometry Library of Dynamic Programming Library of Number Theory</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

セットレンタルを複数回適用することが出来る，とある{% m %} d {% em %}本以上買う場合は(選んだ本数) {% m %} \times e{% em %}となるので，{% m %} 1 {% em %}本を {% m %} e {% em %}円でレンタルすることが出来る訳なので，複数回買ったのを結局まとめてしまえば，セットレンタルをするのは{% m %} 1 {% em %}回で良いことが分かる．  
{% m %} e {% em %}円より高いものはこのセットで買った方がお得で，{% m %} e {% em %}円より高いものが {% m %} d {% em %}本無い場合は，どのへんまでをレンタルしたほうが良いかを判断しなければならないので，値段順に降順ソートして，セットレンタルする範囲と普通に借りる範囲を順番に見て，最小を取った．

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
	ll a, b, c, d, e;
	while(true) {
		cin >> a >> b >> c >> d >> e;

		if(a == 0 && b == 0 && c == 0 && d == 0 && e == 0) break;

		ll na, nb, nc;
		cin >> na >> nb >> nc;

		vector<ll> v;
		rep(i, na) v.push_back(a);
		rep(i, nb) v.push_back(b);
		rep(i, nc) v.push_back(c);

		sort(v.begin(), v.end(), greater<ll>());

		ll sum = a * na + b * nb + c * nc;
		ll ans = sum, pre = 0;

		rep(i, v.size()) {
			pre += v[i];
			sum -= v[i];
			if(i >= d-1) {
				ans = min(ans, sum + min(pre, (i + 1) * e));
			} else {
				ans = min(ans, sum + min(pre, d * e));
			}
		}

		cout << ans << endl;
	}

	return 0;
}
```

