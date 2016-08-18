---
layout: post
title: "AOJ0547 Commute routes"
date: 2016-02-10 22:08:01 +0900
comments: true
categories: [AOJ, 動的計画法]
---

問題文  
http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=0547

<!-- more -->

dp\[i][j] := その地点から横にいける経路の個数  
dp2\[i][j] := その地点から縦にいける経路の個数 とした．  
交差点で曲がった直後が曲がれない場合は，同じ方向であればその経路は存在する．つまりdp\[i]\[j]の場合はdp\[i]\[j-1]，dp2\[i]\[j]の場合はdp\[i-1]\[j]の経路がそのまま存在する．  
また別の方向でも一個交差点を飛ばした場所の経路は存在する．つまりdp\[i]\[j]の場合はdp2\[i-2]\[j]，dp2\[i]\[j]の場合はdp\[i]\[j-2]の経路が存在する．

よって
{% math %}
\begin{eqnarray}
	dp[i][j] &+=& dp[i][j-1] + dp2[i-2][j] \\
	dp2[i][j] &+=& dp[i][j-2] + dp2[i-1][j]
\end{eqnarray}
{% endmath %}

MODを取るのと忘れないようにする．

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
#define MOD 100000

using namespace std;
typedef long long ll;
typedef pair<int,int> P;

int dp[105][105], dp2[105][105];

int main() {
	int w, h;
	while(cin >> w >> h) {
		if(w == 0 && h == 0) break;

		memset(dp, 0, sizeof(dp));
		memset(dp2, 0, sizeof(dp2));

		dp[0][0] = 1;
		dp2[0][0] = 1;

		rep(i, w) {
			dp[0][i] = 1;
			dp2[0][i] = 1;
		}
		
		rep(i, h) {
			dp[i][0] = 1;
			dp2[i][0] = 1;
		}

		REP(i, 1, h) {
			REP(j, 1, w) {
				dp[i][j] += dp[i][j-1];
				if(i - 2 >= 0) dp[i][j] += dp2[i-2][j];
				dp[i][j] %= MOD;
		
				dp2[i][j] += dp2[i-1][j];
				if(j - 2 >= 0) dp2[i][j] += dp[i][j-2];
				dp2[i][j] %= MOD;
			}
		}

		if(h == 2 || w == 2) cout << 2 << endl;
		else cout << (dp[h-1][w-1] + dp[h-2][w-3]) % MOD << endl;
	}
	return 0;
}
```
