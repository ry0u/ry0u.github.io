---
layout: post
title: "AOJ0600 Baumkuchen"
date: 2016-03-10 23:01:54 +0900
comments: true
categories: [AOJ,しゃくとり法]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-type="article" data-card-branding="0"><h4><a href="http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=0600">Baumkuchen | Aizu Online Judge</a></h4><p>Introduction to Programming Introduction to Algorithms and Data Structures Library of Data Structures Library of Graph Algorithms Library of Computational Geometry Library of Dynamic Programming Library of Number Theory</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

円環を切って2つ繋げて表現する．{% m %} sum\[i\]をi {% em %}未満の切り込み間の大きさの和として持っておく．区間{% m %} (s, t\] {% em %}を{% m %}3{% em %}ピースに分けた時の最小値，もう1つの切れ込みを入れる場所を {% m %} id {% em %}と置くと，

* {% m %} (s, t\] {% em %}
* {% m %} (t, id\] {% em %}
* {% m %} (id, s+n\]{% em %}

の{% m %} 3 {% em %}ピースに分けることになる．区間{% m %} (s, t\] {% em %}を最小にしたいので，その区間の大きさ{% m %} d {% em %}を初めて超える場所をlower_boundで取ってくる．後は取ってきた場所から{% m %} s+n {% em %}までの和が{% m %} d {% em %}よりも大きければ良い．もしをこれを満たすようなら，右端を伸ばしてみて，ダメだったら左端を縮める．

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
	int n;
	cin >> n;

	vector<ll> v(n);
	rep(i, n) cin >> v[i];

	vector<ll> sum(2*n + 1);
	rep(i, n) {
		sum[i+1] += sum[i] + v[i];
	}

	rep(i, n) {
		sum[n+i+1] += sum[n + i] + v[i];
	}

	ll ans = 0;
	int s = 0, t = 0,cnt = 0;
	bool flag = true, first = true;

	while(true) {
		if(flag) t++;
		else s++;

		if(s + n >= sum.size()) break;

		ll d = sum[t] - sum[s];
		int id = lower_bound(sum.begin(), sum.end(), sum[t]  + (sum[t] - sum[s])) - sum.begin();

		if(s + n > id && d < sum[s+n] - sum[id]) {
			ans = max(ans, d);
			flag = true;
		} else {
			flag = false;
		}
	}

	cout << ans << endl;

	return 0;
}
```

