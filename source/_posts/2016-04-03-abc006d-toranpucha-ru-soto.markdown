---
layout: post
title: "ABC006D トランプ挿入ソート"
date: 2016-04-03 14:38:50 +0900
comments: true
categories: [ABC, AtCoder, 最長増加部分列]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-branding="0" data-card-type="article"><h4><a href="http://abc006.contest.atcoder.jp/tasks/abc006_4">D: トランプ挿入ソート - AtCoder Beginner Contest 006 | AtCoder</a></h4><p>(null)</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

カードを任意の場所に挿入することが出来るので，最長増加部分列でないカードをそれぞれ操作するのが最小になる．


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

int LIS(vector<int> v) {
	vector<int> ret(v.size() + 1);
	rep(i, v.size()+1) ret[i] = INF;
	rep(i, v.size()) {
		*lower_bound(ret.begin(), ret.end(), v[i]) = v[i];
	}

	return lower_bound(ret.begin(), ret.end(), INF) - ret.begin();
}

int main() {
	int n;
	cin >> n;

	vector<int> v(n);
	rep(i, n) cin >> v[i];

	cout << n - LIS(v) << endl;

	return 0;
}
```

