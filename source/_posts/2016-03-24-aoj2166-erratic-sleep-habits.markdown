---
layout: post
title: "AOJ2166 Erratic Sleep Habits"
date: 2016-03-24 02:13:32 +0900
comments: true
categories: [AOJ-ICPC, 300, 動的計画法]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-type="article" data-card-branding="0"><h4><a href="http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=2166">Erratic Sleep Habits</a></h4><p>Peter is a person with erratic sleep habits. He goes to sleep at twelve o'lock every midnight. He gets up just after one hour of sleep on some days; he may even sleep for twenty-three hours on other days. His sleeping duration changes in a cycle, where he always sleeps for only one hour on the first day of the cycle.</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

睡眠周期 {% m %} T_i {% em %}が与えられる．カフェインを取ることでこの睡眠周期を最初に戻すことが出来るとき，全ての予定をこなせるカフェインの量の最大値を求める．  
{% math %}
	dp[i][j] := i日目に睡眠周期jの時のカフェインの最小値
{% endmath %}
とした． {% m %} j = 0 {% em %}の場合は {% m %} i-1 {% em %}のどこからでも {% m %} +1 {% em %}(カフェインを取ること)で遷移できて，他の場合は {% m %} T_j {% em %}が {% m %} i {% em %}日目の一番最初の予定より早ければ， {% m %} dp[i-1][j-1] {% em %}より遷移できる．

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
	int t;
	while(cin >> t && t) {

		vector<int> v(t);
		rep(i, t) cin >> v[i];

		int n;
		cin >> n;

		vector<int> d(n), m(n);
		int dm[105], day = 0;
		rep(i, 105) dm[i] = INF;

		rep(i, n) {
			cin >> d[i] >> m[i];
			dm[d[i]-1] = min(dm[d[i]-1], m[i]);
			day = max(day, d[i]);
		}

		int dp[105][35];
		rep(i, 105) rep(j, 35) dp[i][j] = INF;

		dp[0][0] = 0;

		REP(i, 1, day) {
			rep(j, t) {
				if(j == 0) {
					rep(k, t) {
						dp[i][j] = min(dp[i][j], dp[i-1][k] + 1);
					}
					dp[i][j] = min(dp[i][j], dp[i-1][t-1]);
				} else {
					if(v[j] <= dm[i]) {
						dp[i][j] = min(dp[i][j], dp[i-1][j-1]);
					}
				}
			}
		}

		int ans = INF;
		rep(i, t) {
			ans = min(ans, dp[day-1][i]);
		}

		cout << ans << endl;
	}

	return 0;
}
```

