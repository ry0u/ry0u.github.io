---
layout: post
title: "AOJ1248 The Balance"
date: 2016-03-24 01:23:32 +0900
comments: true
categories: [AOJ-ICPC, 300]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-type="article" data-card-branding="0"><h4><a href="http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=1248">The Balance</a></h4><p>Ms. Iyo Kiffa-Australis has a balance and only two kinds of weights to measure a dose of medicine. For example, to measure 200mg of aspirin using 300mg weights and 700mg weights, she can put one 700mg weight on the side of the medicine and three 300mg weights on the opposite side (Figure 1).</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

重り {% m %} a {% em %}を使う量を決めると重り{% m %} b {% em %}を使う量が決まる．また重り {% m %} b {% em %}を決めると重り{% m %} a {% em %}を使う量が決まる．それぞれ使う量を全探索してどちらも使う量が非負整数の時に量の和，重さの和の最小を答える．上限を何も考えずに {% m %} 500000 {% em %}まで回したらオーバーフローで死んだ(完)． {% m %} \max{d} {% em %}までで良かった．

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
	int a, b, d;
	while(cin >> a >> b >> d) {
		if(a == 0 && b == 0 && d == 0) break;

		int g = __gcd(a, b);
		a /= g;
		b /= g;
		d /= g;

		int x = INF, y = INF, sum = INF, prod = INF;

		for(int i = 0; i < 50000; i++ ) { 
			int c = d - a * i;
			if(abs(c) % b == 0) {
				int j = abs(c / b);

				if(i + j < sum) {
					sum = i + j;
					prod = a*i + b*j;
					x = i;
					y = j;
				} else if(i + j == sum && a*i + b*j < prod) {
					sum = i + j;
					prod = a*i + b*j;
					x = i;
					y = j;
				}
			}
		}

		for(int j = 0; j < 50000; j++) {
			int c = d - b * j;
			if(abs(c) % a == 0) {
				int i = abs(c / a);
				if(i + j < sum) {
					sum = i + j;
					prod = a*i + b*j;
					x = i;
					y = j;
				} else if(i + j == sum && a*i + b*j < prod) {
					sum = i + j;
					prod = a*i + b*j;
					x = i;
					y = j;
				}
			}
		}

		cout << x << " " << y << endl;
	}
	return 0;
}
```

