---
layout: post
title: "AOJ1306 Balloon Collecting"
date: 2016-03-23 21:01:17 +0900
comments: true
categories: [AOJ-ICPC, 300, 動的計画法]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-type="article" data-card-branding="0"><h4><a href="http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=1306">Balloon Collecting</a></h4><p>"Balloons should be captured efficiently", the game designer says. He is designing an oldfashioned game with two dimensional graphics. In the game, balloons fall onto the ground one after another, and the player manipulates a robot vehicle on the ground to capture the balloons.</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

動的計画法．

{% math %}
	dp[i][j] := i個目の風船をj個集めた時の時間の最小値
{% endmath %}

として， {% m %} j \leq 3 {% em %}の時に風船を取りに行って間に合うならば {% m %} dp[i+1][j+1] {% em %}に遷移可能， 今ある風船を家に置きに行って，次の風船を取りに言って間に合うならば {% m %} dp[i+1][1] {% em %}に遷移可能．  
間に合うかどうかは，そのまま次のを取る{% m %}\to abs (p[i+1] - p[i]) \cdot (j + 1) {% em %}，家に置きに行って風船を次の風船を取る{% m %} \to p[i] \cdot (j+1) + p[i+1] {% em %}が， {% m %} t[i+1] - t[i] {% em %}より小さければ良い．  
家に帰る，と次の風船を取りに行くを別々に考えていてどういう遷移か分からずめちゃくちゃ時間を溶かした．こういう考え方がすぐに出来るようになりたい．

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

int dp[50][4];

int main() {
	int n;
	while(cin >> n && n) {

		vector<int> p(n + 1), t(n + 1);
		rep(i, n) cin >> p[i + 1] >> t[i + 1];

		rep(i, 50) rep(j, 4) dp[i][j] = INF;

		int id = -1;
		dp[0][0] = 0;
		rep(i, n) {
			int d = abs(p[i+1] - p[i]);
			bool flag = true;

			rep(j, 4) {
				if(dp[i][j] == INF) continue;

				if(j < 3 && d * (j + 1) <= t[i+1] - t[i]) {
					flag = false;
					dp[i+1][j+1] = min(dp[i+1][j+1], dp[i][j] + d);
				}

				if(p[i] * (j + 1) + p[i+1] <= t[i+1] - t[i]) {
					flag = false;
					dp[i+1][1] = min(dp[i+1][1], dp[i][j] + p[i] + p[i+1]);
				}
			}

			if(flag) {
				id = i+1;
				break;
			}
		}

		if(id != -1) cout << "NG " << id << endl;
		else {
			int ans = INF;
			rep(j, 4) {
				ans = min(ans, dp[n][j] + p[n]);
			}
			cout << "OK " << ans << endl;
		}
	}

	return 0;
}
```

