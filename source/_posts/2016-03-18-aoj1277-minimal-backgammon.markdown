---
layout: post
title: "AOJ1277 Minimal Backgammon"
date: 2016-03-18 22:14:05 +0900
comments: true
categories: [AOJ-ICPC, 250, 動的計画法, 確率]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-type="article" data-card-branding="0"><h4><a href="http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=1277">Minimal Backgammon</a></h4><p>Here is a very simple variation of the game backgammon, named "Minimal Backgammon". The game is played by only one player, using only one of the dice and only one checker (the token used by the player). The game board is a line of ( N + 1) squares labeled as 0 (the start) to N (the goal).</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

動的計画法．
{% math %}
	dp[i][j] := iターン目にマスjにいる確率
{% endmath %}

とする．{% m %} i {% em %}ターン目にマス{% m %} j {% em %}にいる時にサイコロを振るのは
{% math %}
	dp[i+1][j+k] = dp[i][j] \cdot (1.0 / 6.0);
{% endmath %}
と書ける．一回休みの時は{% m %} dp[i+2][j+k] {% em %}，戻るマスは {% m %} dp[i+1][0] {% em %}に遷移する．{% m %} n {% em %}マスを追い越した時に戻ってくる処理でバグバグした．

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

double dp[105][105];

int main() {
	int n, t, l, b;
	while(cin >> n >> t >> l >> b) {
		if(n == 0 && t == 0 && l == 0 && b == 0) break;

		memset(dp, 0, sizeof(dp));

		bool lose[105], back[1005];
		memset(lose, 0, sizeof(lose));
		memset(back, 0, sizeof(back));

		rep(i, l) {
			int x;
			cin >> x;
			lose[x] = true;
		}

		rep(i, b) {
			int x;
			cin >> x;
			back[x] = true;
		}

		memset(dp, 0, sizeof(dp));

		dp[0][0] = 1.0;
		rep(i, t) {
			rep(j, n) {
				if(dp[i][j] == 0.0) continue;
				REP(k, 1, 7) {
					int p = j + k;
					if(p > n) p = n - (p - n);

					if(lose[p]) {
						dp[i+2][p] += dp[i][j] * (1.0 / 6.0);
					} else if(back[p]) {
						dp[i+1][0] += dp[i][j] * (1.0 / 6.0);
					} else {
						dp[i+1][p] += dp[i][j] * (1.0 / 6.0);
					}
				}
			}
		}

		double ans = 0;
		rep(i, t + 1) {
			ans += dp[i][n];
		}

		cout << fixed;
		cout.precision(20);
		cout << ans << endl;
	}

	return 0;
}
```
