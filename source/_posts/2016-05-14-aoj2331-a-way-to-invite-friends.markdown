---
layout: post
title: "AOJ2331 A Way to Invite Friends"
date: 2016-05-14 23:45:52 +0900
comments: true
categories: [AOJ-ICPC, 250, いもす法]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-branding="0" data-card-type="article"><h4><a href="http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=2331">A Way to Invite Friends | Aizu Online Judge</a></h4><p>Introduction to Programming Introduction to Algorithms and Data Structures Library of Data Structures Library of Graph Algorithms Library of Computational Geometry Library of Dynamic Programming Library of Number Theory</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

各友だちの，嫌がらない海に行く人数の範囲( {% m %} a_i 〜 b_i {% em %})が与えられる．海に誘える最大人数を求める．  
海に行く人数には「わたし」も含まれるので {% m %} i {% em %}人誘えるかは， {% m %} i+1 {% em %}人で海に行くことを嫌がらない友だちの数が {% m %} i {% em %}人以上いるか，となるのでいもす法で累積和を取って順番に見ていった．


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

	int imos[100005];
	memset(imos, 0, sizeof(imos));

	rep(i, n) {
		int a, b;
		cin >> a >> b;

		imos[a]++;
		imos[b+1]--;
	}

	REP(i, 1, 100005) {
		imos[i] += imos[i-1];
	}

	int ans = 0;
	rep(i, 100005) {
		if(imos[i+1] >= i) {
			ans = i;
		}
	}

	cout << ans << endl;

	return 0;
}
```

