---
layout: post
title: "Codeforces353-div2B Restoring Painting"
date: 2016-05-17 17:43:57 +0900
comments: true
categories: [Codeforces, 全探索]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-branding="0" data-card-type="article"><h4><a href="http://codeforces.com/contest/675/problem/B">Problem - B - Codeforces</a></h4><p>Vasya works as a watchman in the gallery. Unfortunately, one of the most expensive paintings was stolen while he was on duty. He doesn't want to be fired, so he has to quickly restore the painting. He remembers some facts about it.</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

　{% m %} 3 \times 3 {% em %}のマスで {% m %} a, b, c, d {% em %}が与えられる． {% m %} 2 \times 2 {% em %}の マスの和が全て等しくなるような ?を埋める {% m %} 1 \sim n {% em %}数字の組はいくつあるか？  
{% img /images/Codeforces/353/b.png %}
  

　?のマスをそれぞれ {% m %} x, y, z, h, m {% em %} と置くと，それぞれ {% m %} 4 {% em %}つの正方形の和は  
{% img /images/Codeforces/353/b2.png %}

1. {% m %} a + b + x + m {% em %}
2. {% m %} a + c + y + m {% em %}
3. {% m %} b + d + z + m {% em %}
4. {% m %} c + d + h + m {% em %}

となり，全て {% m %} m {% em %}が入っていて，{% m %} m {% em %}の値は関係ないことがわかる．なので{% m %} x, y, z, h {% em %}の組の{% m %} n {% em %}倍でいい．  
まず， {% m %} x {% em %}の値を全探索する． {% m %} x = i {% em %}と置くと， {% m %} y, z, h {% em %}が

* {% m %} y = b + x - c {% em %}
* {% m %} z = a + x - d {% em %}
* {% m %} h = a + y - d {% em %}

と決まるので，その値が {% m %} 1 \sim n {% em %}の間に収まっていれば，正しい組となる．その組の場合に {% m %} n {% em %}を足していった({% m %} m {% em %}は何でも良い)．

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
	ll n, a, b, c, d;
	cin >> n >> a >> b >> c >> d;

	ll ans = 0;

	REP(i, 1, n + 1) {
		ll x = i;
		ll y = b + x - c;
		ll z = a + x - d;
		ll h = a + y - d;

		if(1 <= y && y <= n && 1 <= z && z <= n && 1 <= h && h <= n) {
			ans += n;
		}
	}

	cout << ans << endl;

	return 0;
}
```



