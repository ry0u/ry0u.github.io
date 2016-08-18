---
layout: post
title: "AOJ2639 Yamanote Line"
date: 2016-03-22 20:34:06 +0900
comments: true
categories: [AOJ-ICPC, 250]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-type="article" data-card-branding="0"><h4><a href="http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=2639">Yamanote Line | Aizu Online Judge</a></h4><p>Introduction to Programming Introduction to Algorithms and Data Structures Library of Data Structures Library of Graph Algorithms Library of Computational Geometry Library of Dynamic Programming Library of Number Theory</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

起きる，寝るを実際に繰り返す．   

* {% m %} \rm{used}[i][0] :=  起きてる時に時間iに訪れたか{% em %}
* {% m %} \rm{used}[i][1] :=  寝ている時に時間iに訪れたか{% em %}

とする．既に訪れた場所に訪れるとそこでループするので，ループせずに目的の場所に辿り着ける場合は今までの合計の時間，そうではない場合は-1を出力した．

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
	int a, b, c;
	cin >> a >> b >> c;

	bool used[60][2];
	memset(used, 0, sizeof(used));

	int cnt = 0, ans = 0;
	bool flag = true;
	while(true) {
		if(cnt & 1) {
			ans += b;
			if(used[ans%60][1]) {
				flag = false;
				break;
			}
			used[ans%60][1] = true;
		} else {
			if(ans % 60 + a >= 60 && ans % 60 <= c) {
				ans += c - ans % 60;
				break;
			}
			else if(c >= ans % 60 && (ans + a) % 60 >= c) {
				ans += c - ans % 60;
				break;
			} else {
				ans += a;
				if(used[ans%60][0]) {
					flag = false;
					break;
				}
				used[ans%60][0] = true;
			}
		}
		cnt++;
	}

	if(flag) cout << ans << endl;
	else cout << -1 << endl;

	return 0;
}
```

