---
layout: post
title: "Codeforces353-div2C Money Transfers"
date: 2016-05-17 18:20:16 +0900
comments: true
categories: [codeforces, いもす法]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-branding="0" data-card-type="article"><h4><a href="http://codeforces.com/contest/675/problem/C">Problem - C - Codeforces</a></h4><p>There are n banks in the city where Vasya lives, they are located in a circle, such that any two banks are neighbouring if their indices differ by no more than . Also, bank 1 and bank n are neighbours if n  > 1. No bank is a neighbour of itself.</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

隣接した場所の値を自由に交換出来る時，数列を全部{% m %} 0 {% em %}にするには最小何回で出来るか．  
円環なので，切って {% m %} 2 {% em %}つ繋げて表現した．最初は累積和を取って， {% m %} [0, 0] {% em %}となる区間で移動させるようにしていた．これはそれぞれの {% m %} (区間数-1) {% em %}の和が答えで，つまり {% m %} (n - 累積和が0の個数) {% em %}となる．  
累積和が {% m %} 0 {% em %}となる場所の個数は，累積和を開始する地点による．

{% img /images/Codeforces/353/c.png %}

上の図(適当)は，累積和をカウントしたものとする．開始地点をずらすというのは，赤の線をどこに引くか( {% m %} 0 {% em %}となる基準をどこにするか)ということになる．よって，最初の累積和の数の個数を数えて，そのmaxを {% m %} n {% em %}から引く．

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

	vector<ll> v(n);
	rep(i, n) cin >> v[i];


	ll imos[100005];
	memset(imos, 0, sizeof(imos));
	imos[0] = v[0];

	REP(i, 1, n) {
		imos[i] += imos[i-1] + v[i];
	}

	map<ll, int> m;
	int x = 0;

	rep(i, n) {
		m[imos[i]]++;
		x = max(x, m[imos[i]]);
	}

	cout << n - x << endl;

	return 0;
}
```

