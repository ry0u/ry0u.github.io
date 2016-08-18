---
layout: post
title: "Codeforces357-div2B Economy Game"
date: 2016-06-15 23:32:39 +0900
comments: true
categories: [Codeforces]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-branding="0" data-card-type="article"><h4><a href="http://codeforces.com/contest/681/problem/B">Problem - B - Codeforces</a></h4><p>Codeforces. Programming competitions and contests, programming community</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

$a \times 1234567 + b \times 123456 + c \times 1234 = n$となる$a$, $b$, $c$のペアがあるかないかを判定する．  
  
$n \leq 10 ^9$なので最大で$a$は$\frac{10 ^9}{1234567} = 810$, $b$は$\frac{10 ^9}{123456} = 8100$なので  
その数を全探索して，$n - a \times 1234567 - b \times 123456$が$c$で割り切れるかどうかを判定した．

# Code

```cpp
#include <iostream>
#include <sstream>
#include <vector>
#include <string>
#include <cstring>
#include <algorithm>
#include <queue>
#include <map>
#include <set>

#define REP(i,k,n) for(int i=k;i<n;i++)
#define rep(i,n) for(int i=0;i<n;i++)
#define each(it,v) for(__typeof((v).begin()) it=(v).begin();it!=(v).end();it++)
#define INF 1<<30
#define mp make_pair

using namespace std;
typedef long long ll;
typedef pair<int,int> P;

int main() {
	ll n;
	cin >> n;

	ll a = 1234567;
	ll b = 123456;
	ll c = 1234;

	bool flag = false;
	rep(i, 1000) {
		rep(j, 10000) {
			ll x = n - a * i - b * j;
			if(x < 0) break;
			if(x % c == 0) {
				flag = true;
			}
		}
	}

	if(flag) cout << "YES" << endl;
	else cout << "NO" << endl;

	return 0;
}
```
