---
layout: post
title: "Codeforces355-div2B Vanya and Food Processor"
date: 2016-06-02 14:00:25 +0900
comments: true
categories: [Codeforces]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-branding="0" data-card-type="article"><h4><a href="http://codeforces.com/contest/677/problem/B">Problem - B - Codeforces</a></h4><p>Vanya smashes potato in a vertical food processor. At each moment of time the height of the potato in the processor doesn't exceed h and the processor smashes k centimeters of potato each second. If there are less than k centimeters remaining, than during this second processor smashes all the remaining potato.</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

高さ{% m %} h {% em %}，{% m %} 1 {% em %}秒間に{% m %} k {% em %}を潰せるフードプロセッサーがある．高さ {% m %} a_i {% em %}のじゃがいもが {% m %} n {% em %}個ある時に何秒で全て潰すことが出来るか？(indexの小さい順に崩していく)．  
潰す順番を変更出来ないので，単純に入れて潰す． {% m %} mod\ k\ +\ a_i \leq h{% em %}の時には次のを入れて潰す．そうでないの時は潰してから入れる．

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
#include <stack>

#define REP(i,k,n) for(int i=k;i<n;i++)
#define rep(i,n) for(int i=0;i<n;i++)
#define INF 1<<30
#define pb push_back
#define mp make_pair

using namespace std;
typedef long long ll;
typedef pair<int,int> P;

int main() {
	ll n;
	cin >> n;

	ll k, h;
	cin >> h >> k;

	vector<ll> v(n);
	rep(i, n) cin >> v[i];

	ll ans = 0, sum = 0;
	rep(i, n) {
		if(sum + v[i] <= h) sum += v[i];
		else {
			sum = v[i];
			ans++;
		}

		ans += sum / k;
		sum %= k;
	}

	if(sum != 0) ans++;

	cout << ans << endl;

	return 0;
}
```

本番は順番を変更出来るものだと勘違いしていて，pretestが延々と通らずに泣いていた．順番が変更出来る場合は，大きいものから入れれるなら入れて，入りきらなくなったら潰す．を繰り返していたのですが，この貪欲はあっているのかわからない．
