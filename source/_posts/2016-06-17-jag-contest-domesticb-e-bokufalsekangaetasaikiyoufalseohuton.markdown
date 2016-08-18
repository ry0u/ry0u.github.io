---
layout: post
title: "JAG Contest 2016 DomesticB E ぼくのかんがえたさいきょうのおふとん"
date: 2016-06-17 17:23:13 +0900
comments: true
categories: [模擬予選, 動的計画法]
---

<!-- more -->

最適なおふとんの並べ方が分かっていたとすると，おふとんを一番上から連続して使うことしか出来ないので，$M$日間のぬくもり需要$d_j$の順番は関係なくなる．sort後は$d\_i < d\_{i+1}$となっていて，$d\_{i+1}$の最小は$d_i$で最小であったおふとんの並び方に新しくおふとんを追加するかしないか，となる．

$$
	dp[i][j] := i日目まででおふとん集合jの時の最小値
$$

として，動的計画法．集合$j$の部分集合のminと考えるのではなく，集合$j$に要素を一個加えたものを更新する．

## Code

```cpp
#include <iostream>
#include <vector>
#include <string>
#include <cstring>
#include <algorithm>
#include <bitset>

#define REP(i, k, n) for(int i = k; i < n; i++)
#define rep(i, n) for(int i = 0; i < n; i++)
#define INF 1<<30

using namespace std;
typedef long long ll;

ll dp[105][1<<15];
ll res[1<<15];

int main() {
	int n, m;
	while(cin >> n >> m) {
		if(n == 0 && m == 0) break;

		vector<ll> s(n), d(m);
		rep(i, n) cin >> s[i];
		rep(i, m) cin >> d[i];

		sort(d.begin(), d.end());

		rep(i, 105) rep(j, 1<<15) dp[i][j] = INF;
		rep(j, 1<<15) dp[0][j] = 0;

		rep(i, m) {
			ll val = INF;
			rep(j, 1<<n) {
				ll sum = 0;
				rep(k, n) {
					if(j & (1<<k)) {
						sum += s[k];
					}
				}

				rep(k, n) {
					if(j & (1<<k)) continue;
					dp[i][j | (1<<k)] = min(dp[i][j | (1<<k)], dp[i][j]);
				}

				dp[i+1][j] = min(dp[i+1][j], dp[i][j] + abs(d[i] - sum));
			}
		}

		ll ans = INF;
		rep(j, 1<<n) {
			ans = min(ans, dp[m][j]);
		}

		cout << ans << endl;
	}
	return 0;
}
```

