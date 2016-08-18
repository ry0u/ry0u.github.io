---
layout: post
title: "AOJ1077 The Great Summer Contest"
date: 2016-05-16 08:29:30 +0900
comments: true
categories: [AOJ, Vol10]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-branding="0" data-card-type="article"><h4><a href="http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=1077">The Great Summer Contest | Aizu Online Judge</a></h4><p>Introduction to Programming Introduction to Algorithms and Data Structures Library of Data Structures Library of Graph Algorithms Library of Computational Geometry Library of Dynamic Programming Library of Number Theory</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

それぞれの分野の問題が与えられるので，最大何回コンテストを開けるかを答える．  
最初は合わせて {% m %} 3 {% em %}問，ということでどちらかは必ず無いとダメと勘違いしていた．そうではないのでMathとDP，GreedyとGraph，GeometryとOtherの区別が入らない．バランスの良いコンテストを {% m %} 3 {% em %}回行うのと，それぞれのコンテストを {% m %} 3 {% em %}回やるのは同じであるが， バランスの良いコンテストを行えるだけ行う場合では，それぞれの分野のコンテストを行ったほうが良い場合がある．  例えば {% m %} (1, 3, 3) {% em %}では，バランスの良いコンテストは {% m %} 1 {% em %}回行えるがそれぞれの分野のコンテストは {% m %} 2 {% em %}回行える．なので {% m %} Max( {% em %}バランスの良いコンテスト行えるだけ行ったもの, それから {% m %} 1 or 2 {% em %}段階戻して，それぞれの分野のコンテストを行ったもの {% m %} ) {% em %}を取った．


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
	ll a, b, c, d, e, f;
	while(cin >> a >> c >> e >> b >> d >> f) {
		if(a == 0 && b == 0 && c == 0 && d == 0 && e == 0 && f == 0) break;

		ll ans = 0, A = 0, B = 0, C = 0;
		ll res = min(a + b, min(c + d, e + f));

		A = (a + b) - res;
		B = (c + d) - res;
		C = (e + f) - res;
		ans = res + (A / 3) + (B / 3) + (C / 3);

		if(res >= 1) {
			A++; B++; C++;
			res--;
		}
		else if(res >= 2) {
			A += 2; B += 2; C += 2;
			res -= 2;
		}

		cout << max(ans, res + (A / 3) + (B / 3) + (C / 3)) << endl;
	}
	return 0;
}
```

