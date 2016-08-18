---
layout: post
title: "AOJ0573 Night Market"
date: 2016-02-16 23:20:29 +0900
comments: true
categories: [AOJ,動的計画法]
---

問題文  
http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=0573  

<!-- more -->

ずっとグラフだろうと考えていた．
頂点を(楽しさ，時間)で持って，indexが大きい頂点にコストを時間とした辺を貼ったグラフを作っても，何を優先度にしてどう探索するか分からない．  
最短時間を求めてながら楽しさを更新しても，時間が長く楽しさが非常に高い辺が来たら答えが合わないことに気づき，ダメ．  


時間に注目してみた．ある時間での楽しさの最大値を考えるとナップサックのDPになった．indexの小さい順に訪れるというのは，与えられた順に{% m %} i{% em %}番目のお店以内，と考えれば解決で，花火の時間について時刻{% m %} S {% em %}をまたがる遷移をやめれば良さそうである． 

{% math %}
	dp[i][j] := i番目以内のお店で，時刻jまで遊んだ時の楽しさの最大値
{% endmath %}
として，花火の時間をまたがる遷移をやめ，その最大値を求めたらACした．

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
#define mpp(t1, t2, t3, t4) mp(mp(t1, t2), mp(t3, t4))

using namespace std;
typedef long long ll;
typedef pair<int,int> P;
typedef pair<P, P> PP;

int dp[3005][3005];

int main() {
	int n, t, s;
	cin >> n >> t >> s;

	vector<P> v(n);
	rep(i, n) {
		cin >> v[i].first >> v[i].second;
	}

	memset(dp, 0, sizeof(dp));

	REP(i, 0, n) {
		rep(j, t + 1) {
			if(j - v[i].second < 0 || (j - v[i].second < s && s < j) ) {
				dp[i+1][j] = dp[i][j];
			} else {
				dp[i+1][j] = max(dp[i][j], dp[i][j-v[i].second] + v[i].first);
			}
		}
	}

	int ans = 0;
	rep(i, n + 1 ) {
		rep(j, t + 1) {
			ans = max(ans, dp[i][j]);
		}
	}

	cout << ans << endl;

	return 0;
}
```

