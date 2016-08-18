---
layout: post
title: "AOJ1286 Expected Allowance"
date: 2016-03-18 22:54:03 +0900
comments: true
categories: [AOJ-ICPC, 250, 動的計画法, 期待値]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-type="article" data-card-branding="0"><h4><a href="http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=1286">Expected Allowance | Aizu Online Judge</a></h4><p>Introduction to Programming Introduction to Algorithms and Data Structures Library of Data Structures Library of Graph Algorithms Library of Computational Geometry Library of Dynamic Programming Library of Number Theory</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

{% math %}
	dp[i][j] := i回，m面のサイコロを降った時にjが出る回数
{% endmath %}

として，シュミレーション．サイコロを振るのは {% m %} dp[i+1][j+k] += dp[i][j] {% em %}と書ける．配列を再利用するために，{% m %} iとi+1 {% em %}の偶奇を見て遷移する．次に遷移する場所に値が残っているとおかしいことになるので，{% m %} dp[i][j] {% em %}からサイコロを振ったらそこは初期化する．分母は全て {% m %} m ^n {% em %}で， {% m %} k {% em %}引いた時に最低でも {% m %} 1 {% em %}になるようにして期待値を求める．

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

int dp[2][100005];

int main() {
	int n, m, k;
	while(cin >> n >> m >> k) {
		if(n == 0 && m == 0 && k == 0) break;

		memset(dp, 0, sizeof(dp));
		REP(j, 1, m + 1) {
			dp[1][j] = 1;
		}

		rep(i, n) {
			rep(j, 100005) {
				if(dp[i & 1][j] == 0) continue;
				REP(k, 1, m+1) {
					dp[(i+1)&1][j+k] += dp[i&1][j];
				}
				dp[i&1][j] = 0;
			}
		}

		double ans = 0, t = 1;
		rep(i, n) {
			t *= m;
		}

		rep(j, 100005) {
			if(dp[n&1][j] == 0) continue;

			double l = j - k;
			if(l <= 0) l = 1;
			ans += (dp[n & 1][j] / t) * l;
		}

		cout << fixed;
		cout.precision(20);
		cout << ans << endl;
	}

	return 0;
}
```

