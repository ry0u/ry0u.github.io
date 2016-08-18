---
layout: post
title: "AOJ2369 CatChecker"
date: 2016-03-23 20:34:54 +0900
comments: true
categories: [AOJ-ICPC, 300, メモ化再帰]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-type="article" data-card-branding="0"><h4><a href="http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=2369">CatChecker | Aizu Online Judge</a></h4><p>Introduction to Programming Introduction to Algorithms and Data Structures Library of Data Structures Library of Graph Algorithms Library of Computational Geometry Library of Dynamic Programming Library of Number Theory</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

区間 {% m %} \[l, r\] {% em %}がねこ鳴き声か探索する．区間の端が {% m %} m {% em %}， {% m %} w {% em %}で真ん中の {% m %} e {% em %}で区切ってみる．メモ化しないとTLEした．逆からやると一意に決まる系かと思ったけどそんなことなかった．

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

string s;
int memo[505][505];

bool dfs(int l, int r) {
	if(l > r) return true;
	if(memo[l][r] != -1) return memo[l][r];

	if(s[l] == 'm' && s[r] == 'w') {
		REP(i, l+1, r) {
			if(s[i] == 'e') {
				if(dfs(l+1, i-1) && dfs(i+1, r-1)) {
					return memo[l][r] = true;
				}
			}
		}
	}
	return memo[l][r] = false;
}

int main() {
	cin >> s;
	memset(memo, -1, sizeof(memo));

	if(dfs(0, s.size()-1)) {
		cout << "Cat" << endl;
	} else {
		cout << "Rabbit" << endl;
	}

	return 0;
}
```

