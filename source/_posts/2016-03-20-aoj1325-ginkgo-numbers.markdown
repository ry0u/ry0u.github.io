---
layout: post
title: "AOJ1325 Ginkgo Numbers"
date: 2016-03-20 23:11:37 +0900
comments: true
categories: [AOJ-ICPC, 250, 数学]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-type="article" data-card-branding="0"><h4><a href="http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=1325">Ginkgo Numbers</a></h4><p>We will define Ginkgo numbers and multiplication on Ginkgo numbers. A Ginkgo number is a pair where m and n are integers. For example, , and are Ginkgo numbers. A Ginkgo number is called a prime if m 2+ n 2 > 1 and it has exactly eight divisors.</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

与えられている {% m %} 2 {% em %}個目の性質だけを使った． {% m %} m ^2 + n ^2 {% em %}を割り切れる数が {% m %} x ^2 + y ^2 {% em %}という形で表せるか，という問題になり

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-type="article" data-card-branding="0"><h4><a href="http://mathtrain.jp/twosquare">フェルマーの二平方和定理 | 高校数学の美しい物語</a></h4><p>整数論の美しい定理であるフェルマーの二平方和定理を解説します。平方剰余などの議論を用いた証明。</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

この記事通りに，素因数の{% m %} 4k + 3 {% em %}型の指数が全て偶数かどうかを見て判断する． {% m %} 1 {% em %}と {% m %} m ^2 + n ^2 {% em %}以外に存在するか，ということで合計して{% m %} 3 {% em %}つ以上あれば{% m %} C {% em %}となる．


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

map<ll,ll> prime_factor(ll n) {
	map<ll,ll> res;
	for(ll i=2; i*i <= n; i++) {
		while(n%i == 0) {
			res[i]++;
			n /= i;
		}
	}

	if(n != 1) res[n] = 1;
	return res;
}

int main() {
	int n;
	cin >> n;

	rep(i, n) {
		int m, n;
		cin >> m >> n;

		int s = m * m + n * n;
		int cnt = 0;

		REP(i, 1, s + 1) {
			if(s % i == 0) {
				map<ll, ll> ret = prime_factor(s / i);
				map<ll, ll>::iterator ite;
				bool flag = true;
				for(ite = ret.begin(); ite != ret.end(); ite++) {
					if(ite->first  % 4 != 3) continue;
					if(ite->second % 2 == 0) continue;
					flag = false;
				}

				if(flag) {
					cnt++;
				}
			}
		}

		if(cnt >= 3) cout << "C" << endl;
		else cout << "P" << endl;
	}

	return 0;
}
```

