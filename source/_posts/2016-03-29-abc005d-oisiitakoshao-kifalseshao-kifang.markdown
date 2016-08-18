---
layout: post
title: "ABC005D おいしいたこ焼きの焼き方"
date: 2016-03-29 23:17:17 +0900
comments: true
categories: [AtCoder, ABC, 累積和]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-branding="0" data-card-type="article"><h4><a href="http://abc005.contest.atcoder.jp/tasks/abc005_4">D: おいしいたこ焼きの焼き方 - AtCoder Beginner Contest 005 | AtCoder</a></h4><p>(null)</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

たこ焼きの美味しさの累積和を取っておく． 
{% math %}
	cnt[i][j] := (0, 0) 〜 (i, j)までのたこ焼きの美味しさの和
{% endmath %}
とすると，範囲 {% m %} (i, j) 〜 (i+k, j+l){% em %}は   

* 個数 : {% m %} (k + 1) \cdot (l + 1) {% em %}
* 美味しさ : {% m %} cnt[i+k][j+l] - cnt[i-1][j+l] - cnt[i+k][j-1] + cnt[i-1][j-1] {% em %}

となるので，後はこれを {% m %} map(個数,美味しさ) {% em %}に突っ込む．後は たこ焼きの個数{% m %} p_i {% em %}以下の最大の美味しさを求める． {% m %} O(N ^4 log N ^2) {% em %}．

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

	ll cnt[55][55];
	memset(cnt, 0, sizeof(cnt));

	rep(i, n) {
		rep(j, n) {
			cin >> cnt[i][j];
		}
	}

	rep(i, n) {
		REP(j, 1, n) {
			cnt[i][j] += cnt[i][j-1];
		}
	}

	REP(i, 1, n) {
		rep(j, n) {
			cnt[i][j] += cnt[i-1][j];
		}
	}

	map<int, int> m;
	rep(i, n) {
		rep(j, n) {
			rep(k, n) {
				if(i + k >= n) continue;
				rep(l, n) {
					if(j + l >= n) continue;

					int res = cnt[i+k][j+l];

					if(i - 1 >= 0) {
						res -= cnt[i-1][j+l];
					}

					if(j - 1 >= 0) {
						res -= cnt[i+k][j-1];
					}

					if(i - 1 >= 0 && j - 1 >= 0) {
						res += cnt[i-1][j-1];
					}

					m[(k + 1) * (l + 1)] = max(m[(k + 1) * (l + 1)] , res);
				}
			}
		}
	}

	int Q;
	cin >> Q;

	rep(q, Q) {
		int p;
		cin >> p;

		int ans = 0;
		rep(i, p + 1) {
			ans = max(ans, m[i]);
		}

		cout << ans << endl;
	}

	return 0;
}
```

