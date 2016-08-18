---
layout: post
title: "AOJ2243 Step Step Evolution"
date: 2016-03-20 22:50:10 +0900
comments: true
categories: [AOJ-ICPC, 250, 実装]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-type="article" data-card-branding="0"><h4><a href="http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=2243">Step Step Evolution</a></h4><p>Japanese video game company has developed the music video game called Step Step Evolution. The gameplay of Step Step Evolution is very simple. Players stand on the dance platform, and step on panels on it according to a sequence of arrows shown in the front screen.</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

左右交互に踏めなくなった回数を数える．最初に左足，右足の {% m %} 2 {% em %}パターンの{% m %} min {% em %}を取る． {% m %} \rm{mod} 3{% em %} で場合分けをしたが， {% m %} \rm{mod} 2 {% em %}になっている箇所があることにずっと気づかずに時間を溶かした．気をつけたい．

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

bool can(bool f, int cur, int to) {
	if(f) {
		if(cur % 3 == 1 && to % 3 != 1) return false;
		if(cur % 3 == 2 && to % 3 == 0) return false;
		return true;
	} else {
		if(cur % 3 == 0 && to % 3 != 0) return false;
		if(cur % 3 == 2 && to % 3 == 1) return false;
		return true;
	}
}

int main() {
	string s;
	while(cin >> s) {
		if(s == "#") break;

		vector<int> v(s.size());
		rep(i, s.size()) v[i] = s[i] - '0';

		int ans = INF, res = 0;
		int left = v[0], right = 0, ord = 0;

		REP(i, 1, v.size()) {
			if(ord & 1) {
				if(can(1, right, v[i])) {
					left = v[i];
					ord++;
				} else {
					right = v[i];
					res++;
				}
			} else {
				if(can(0, left, v[i])) {
					right = v[i];
					ord++;
				} else {
					left = v[i];
					res++;
				}
			}
		}

		ans = min(ans, res);

		res = 0;
		left = 0, right = v[0], ord = 1;
		REP(i, 1, v.size()) {
			if(ord & 1) {
				if(can(1, right, v[i])) {
					left = v[i];
					ord++;
				} else {
					right = v[i];
					res++;
				}
			} else {
				if(can(0, left, v[i])) {
					right = v[i];
					ord++;
				} else {
					left = v[i];
					res++;
				}
			}
		}

		ans = min(ans, res);
		cout << ans << endl;
	}
	return 0;
}
```

