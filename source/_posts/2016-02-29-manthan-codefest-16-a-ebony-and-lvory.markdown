---
layout: post
title: "Manthan, Codefest 16A Ebony and lvory"
date: 2016-02-29 10:27:42 +0900
comments: true
categories: [Codeforces, 動的計画法]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-chrome="0" data-card-controls="0"><h4><a href="http://codeforces.com/contest/633/problem/A">Problem - A - Codeforces</a></h4><p>Dante is engaged in a fight with "The Savior". Before he can fight it with his sword, he needs to break its shields. He has two guns, Ebony and Ivory, each of them is able to perform any non-negative number of shots.</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more-->

{% math %}
	a, bを用いてcを表せるか．使用回数は0でも良い．
{% endmath %}

{% math %}
	iが表せるならi+a, i+bも表すことが出来る．O(\max(c))
{% endmath %}

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

bool used[10500];

int main() {
	int a, b, c;
	cin >> a >> b >> c;

	memset(used, 0, sizeof(used));
	used[0] = true;

	rep(i, c + 1) {
		if(used[i]) {
			used[i+a] = true;
		}
	}

	rep(i, c + 1) {
		if(used[i]) {
			used[i+b] = true;
		}
	}

	if(used[c]) cout << "Yes" << endl;
	else cout << "No" << endl;

	return 0;
}
```
