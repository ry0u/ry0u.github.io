---
layout: post
title: "Codeforces352-div2D Robin Hood"
date: 2016-05-12 15:14:01 +0900
comments: true
categories: [Codeforces, 2分探索]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-branding="0" data-card-type="article"><h4><a href="http://codeforces.com/contest/672/problem/D">Problem - D - Codeforces</a></h4><p>We all know the impressive story of Robin Hood. Robin Hood uses his archery skills and his wits to steal the money from rich, and return it to the poor. There are n citizens in Kekoland, each person has coins.</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>


<!-- more -->

　{% m %} N {% em %}人いて，コインを {% m %} c_i {% em %}枚持っている． {% m %} k {% em %}回，ある人からコインを取って，ある人に挙げれる時，商人のコインの最大値と最小値の差を最小化する．  
  

---

全員が持つコインの枚数を{% m %} 2 {% em %}分探索．最大値と最小値の差を最大化したいので，最大値の最小化と，最小値の最大化を取って差を取る．コインの枚数を {% m %} mid {% em %}と決めた時に， {% m %} mid {% em %}より(多い / 少ない)コインの枚数が {% m %} k {% em %}枚以下かどうかが条件となる．  
全てのコインの合計が{% m %} N {% em %}人で割り切れない時は，必ず余りが出るので，その差 {% m %} +1 {% em %}が答えになる．

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
	cin.tie(0);
	ios::sync_with_stdio(false);

	int n, k;
	cin >> n >> k;

	vector<ll> v(n);
	ll lv = INF, rv = 0;
	ll sum = 0;
	rep(i, n) {
		cin >> v[i];
		sum += v[i];
		lv = min(lv, v[i] - 1);
		rv = max(rv, v[i] + 1);
	}

	sort(v.begin(), v.end());

	ll l = lv, r = rv;
	ll vmax = 0, vmin = INF;
	while(r - l > 1) {
		ll mid = (l + r) / 2;
		ll cnt = 0;
		rep(i, n) {
			if(v[i] > mid) {
				cnt += v[i] - mid;
			}
		}

		if(cnt <= k) {
			vmin = min(vmin, mid);
			r = mid;
		} else {
			l = mid;
		}
	}

	l = lv, r = rv;
	while(r - l > 1) {
		ll mid = (l + r) / 2;
		ll cnt = 0;
		rep(i, n) {
			if(v[i] < mid) {
				cnt += mid - v[i];
			}
		}

		if(cnt <= k) {
			vmax = max(vmax, mid);
			l = mid;
		} else {
			r = mid;
		}
	}

	if(sum % n == 0) {
		cout << max(0LL, vmin - vmax) << endl;
	} else {
		cout << max(1LL, vmin - vmax) << endl;
	}

	return 0;
}
```

