---
layout: post
title: "AOJ2383 Rabbit Game Playing"
date: 2016-05-30 21:06:32 +0900
comments: true
categories: [AOJ-ICPC, 350]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-branding="0" data-card-type="article-full"><h4><a href="http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=2383">Rabbit Game Playing | Aizu Online Judge</a></h4><p>Introduction to Programming Introduction to Algorithms and Data Structures Library of Data Structures Library of Graph Algorithms Library of Computational Geometry Library of Dynamic Programming Library of Number Theory</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

現在プレイした(ステージの難易度 {% m %} -T {% em %})以上の難易度のステージをプレイ出来る． {% m %} N {% em %}ステージ全てプレイする時に，何通りの方法があるか？ {% m %} mod\ 1000000007 {% em %}で求める．  
  
難易度でsortして，小さい順に列に入れていく． {% m %} i-1個 {% em %}で構成される上記のルールを満たす列に {% m %} v[i] {% em %}を入れることを考える．列の中に {% m %} v[i]-t {% em %}以上のものがあれば，その後に {% m %} v[i] {% em %}を入れることが出来る．つまり， ({% m %} i-1 {% em %}までの場合の数 {% m %} \times {% em %} {% m %} iまででv[i]-t {% em %}以上のステージの個数)通りとなる．これを {% m %} N {% em %}回繰り返す．

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
#define MOD 1000000007

using namespace std;
typedef long long ll;
typedef pair<int,int> P;

int main() {
	int n, t;
	cin >> n >> t;

	vector<int> v(n);
	rep(i, n) cin >> v[i];

	sort(v.begin(), v.end());

	ll ans = 1;
	vector<int> res;
	rep(i, n) {
		res.push_back(v[i]);
		int id = lower_bound(res.begin(), res.end(), v[i] - t) - res.begin();
		ans *= (res.size() - id);
		ans %= MOD;
	}

	cout << ans << endl;

	return 0;
}
```
