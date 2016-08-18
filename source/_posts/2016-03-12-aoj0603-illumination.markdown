---
layout: post
title: "AOJ0603 Illumination"
date: 2016-03-12 21:51:55 +0900
comments: true
categories: [AOJ]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-type="article" data-card-branding="0"><h4><a href="http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=0603">Illumination | Aizu Online Judge</a></h4><p>Introduction to Programming Introduction to Algorithms and Data Structures Library of Data Structures Library of Graph Algorithms Library of Computational Geometry Library of Dynamic Programming Library of Number Theory</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

まんまこれだ
<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-type="article" data-card-branding="0"><h4><a href="http://codeforces.com/contest/604/problem/C">Problem - C - Codeforces</a></h4><p>Kevin has just recevied his disappointing results on the USA Identification of Cows Olympiad (USAICO) in the form of a binary string of length n. Each character of Kevin's string represents Kevin's score on one of the n questions of the olympiad-'1' for a correctly identified cow and '0' otherwise.</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

と思ったけど，連続，非連続で違った．  
連続して{% m %} 01 {% em %}，または{% m %} 10 {% em %}の列に分ける．{% m %} 1 {% em %}つの列を反転するとその前後の列を含む{% m %} 01, 10 {% em %}列になる．

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

	vector<int> v(n);
	rep(i, n) cin >> v[i];

	vector<int> t;
	t.push_back(0);
	REP(i, 1, n) {
		if(v[i-1] == v[i]) {
			t.push_back(i);
		}
	}
	t.push_back(n);

	int ans = 0;
	rep(i, t.size()-1) {
		ans = max(ans, t[i+1] - t[i]);
	}

	rep(i, n) {
		int j = upper_bound(t.begin(), t.end(), i) - t.begin();
		int res = t[j+1] - t[j-2];
		ans = max(ans, res);
	}

	cout << ans << endl;

	return 0;
}
```
