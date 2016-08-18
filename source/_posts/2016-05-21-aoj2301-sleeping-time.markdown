---
layout: post
title: "AOJ2301 Sleeping Time"
date: 2016-05-21 03:02:32 +0900
comments: true
categories: [AOJ-ICPC, 350]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-branding="0" data-card-type="article"><h4><a href="http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=2301">Sleeping Time | Aizu Online Judge</a></h4><p>Miki is a high school student. She has a part time job, so she cannot take enough sleep on weekdays. She wants to take good sleep on holidays, but she doesn't know the best length of sleeping time for her.</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

愚直に {% m %} k {% em %}回のシュミレーションに対して，間違う/間違わないをすると {% m %} 2 ^{30} {% em %}となる．ここで枝刈りを行う．シュミレーションが2分探索みたいな感じなので，一度解区間 {% m %} [T-E, T+E] {% em %}を外れたら，再度解区間に入ることは無いので切って良い．この枝刈りだけで {% m %} 04.91s {% em %}でACが取れた．

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

int k;
double p, e, t;

double ans = 0;

void dfs(double l, double r, int cnt, double x) {
	if(r < t - e) return;
	if(l > t + e) return;

	double h = (l + r) / 2;

	if(cnt == k) {
		if(t - e <= h && h <= t + e) ans += x;
		return;
	}

	if(h >= t) {
		dfs(l, h, cnt + 1, (1.0 - p) * x);
		dfs(h, r, cnt + 1, p * x);
	} else {
		dfs(h, r, cnt + 1, (1.0 - p) * x);
		dfs(l, h, cnt + 1, p * x);
	}
}

int main() {
	double l, r;
	cin >> k >> l >> r;
	cin >> p >> e >> t;

	dfs(l, r, 0, 1.0);

	cout << fixed;
	cout.precision(20);
	cout << ans << endl;

	return 0;
}
```

しかし，他の人の解答を見てみると{% m %} 0.0s {% em %}ばかりだった．自分の解法は {% m %} E {% em %}が十分小さい時に有効な枝刈りで， {% m %} E {% em %}が大きい場合を考えていなかった． この時は解区間が探索区間を内包しているので，この先解区間を出ることはない．よってその枝刈りを追加すると {% m %} 0.0s {% em %}とめちゃくちゃ早くなった．

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

int k;
double p, e, t;

double ans = 0;

void dfs(double l, double r, int cnt, double x) {
	if(r < t - e) return;
	if(l > t + e) return;
	if(t - e <= l && r <= t + e) {
		ans += x;
		return;
	}

	double h = (l + r) / 2;

	if(cnt == k) {
		if(t - e <= h && h <= t + e) ans += x;
		return;
	}

	if(h >= t) {
		dfs(l, h, cnt + 1, (1.0 - p) * x);
		dfs(h, r, cnt + 1, p * x);
	} else {
		dfs(h, r, cnt + 1, (1.0 - p) * x);
		dfs(l, h, cnt + 1, p * x);
	}
}

int main() {
	double l, r;
	cin >> k >> l >> r;
	cin >> p >> e >> t;

	dfs(l, r, 0, 1.0);

	cout << fixed;
	cout.precision(20);
	cout << ans << endl;

	return 0;
}
```

まとめると，    

* {% m %} E {% em %}が十分小さい時に，解区間から外れた場合の枝刈りが有効
* {% m %} E {% em %}が十分大きい時に，解区間が内包する場合の枝刈りが有効

となって， {% m %} 2 ^{30} {% em %}にはならないで間に合う．(どのくらい減るのかよく分かっていない)

