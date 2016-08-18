---
layout: post
title: "AOJ0572 Card Game is Fun"
date: 2016-02-16 23:08:35 +0900
comments: true
categories: [AOJ, しゃくとり法]
---

問題文  
http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=0572

<!-- more -->

ブルーノは上から何枚か，下から何枚かを削る．つまり連続したカードが残る．もしブルーノのカードの山が決まった時に，アンナがそのカードの山を作れるかは，先頭から見ていきアンナの山にあるかどうかを見れば良い．  
ブルーノの残すと考えてる暫定カードの先頭と末尾の{% m %}id{% em %}を持っておき，  
そのカードの山をアンナが作れるのであれば，末尾を{% m %}+1もしそうでなければ先頭を+1する． {% em %}  
この{% m %}maxを取れば良いのでO(n ^2){% em %}．
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
	int n, m;
	cin >> n >> m;

	vector<int> a(n), b(m);
	rep(i, n) cin >> a[i];
	rep(i, m) cin >> b[i];

	set< vector<int> > st;

	int ans = 0,s = 0, t = 1;
	while(s != b.size()) {
		vector<int> v(t - s);
		REP(j, s, t) {
			v[j-s] = b[j];
		}

		bool flag = false;
		int id = 0;
		rep(j, n) {
			if(a[j] == v[id]) {
				id++;
				if(id == v.size()) {
					flag = true;
					break;
				}
			}
		}

		if(flag) {
			t++;
			ans = max(ans, (int)v.size());
		} else {
			s++;
			if(s == t) t++;
		}

	}

	cout << ans << endl;

	return 0;
}
```

