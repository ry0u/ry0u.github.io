---
layout: post
title: "AOJ1074 Poplularity Estimation"
date: 2016-05-12 01:15:13 +0900
comments: true
categories: [AOJ, Vol10]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-branding="0" data-card-type="article-full"><h4><a href="http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=1074">Popularity Estimation | Aizu Online Judge</a></h4><p>Introduction to Programming Introduction to Algorithms and Data Structures Library of Data Structures Library of Graph Algorithms Library of Computational Geometry Library of Dynamic Programming Library of Number Theory</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

最初は各時間に名前を突っ込んで，時間で見て各キャラクターにポイントを配る方式にしてWAを量産した．間違っている所を発見することができず(諦めの気持ちで)，{% m %} cnt[i] (時間i {% em %}にいた人数)として，各キャラクターで見ていった結果普通にACが取れた．ポイントについては3行で場合分けが書いてあるけど，1行で{% m %} p[s[i]] += n - cnt[d[i][j]] + 1 {% em %}とかけた．  

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
	while(cin >> n && n) {
		map<string, int> p;
		int cnt[35];
		memset(cnt, 0, sizeof(cnt));

		string s[50];
		int m[50], d[50][50];
		rep(i, n) {
			cin >> s[i] >> m[i];

			rep(j, m[i]) {
				cin >> d[i][j];
				cnt[d[i][j]]++;
			}
		}

		rep(i, n) {
			rep(j, m[i]) {
				p[s[i]] += n - cnt[d[i][j]] + 1;
			}
		}

		int ans = INF;
		string res = "";
		rep(i, n) {
			if(ans > p[s[i]]) {
				ans = p[s[i]];
				res = s[i];
			} else if(ans == p[s[i]] && res > s[i]) {
				ans = p[s[i]];
				res = s[i];
			}
		}

		cout << ans << " " << res << endl;
	}

	return 0;
}
```

