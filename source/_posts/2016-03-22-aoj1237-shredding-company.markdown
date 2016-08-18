---
layout: post
title: "AOJ1237 Shredding Company"
date: 2016-03-22 17:04:43 +0900
comments: true
categories: [AOJ-ICPC, 250, 全探索]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-type="article" data-card-branding="0"><h4><a href="http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=1237">Shredding Company</a></h4><p>You have just been put in charge of developing a new shredder for the Shredding Company. Although a ``normal'' shredder would just shred sheets of paper into little pieces so that the contents would become unreadable, this new shredder needs to have the following unusual basic characteristics.</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more--> 

作れる数を全列挙して， {% m %} n {% em %}以下の最大の数を出力する．構成する数も出力するので，dfsの引数ににvectorを持たせてみた．

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

int stoa(string s) {
	int ret = 0, t = 1;
	for(int i = s.size()-1; i >= 0; i--) {
		ret += (s[i] - '0') * t;
		t *= 10;
	}
	return ret;
}

int n;
string s;
int vmin = INF, vmax = 0, ans = 0, cnt = 0;
vector<int> res;

void dfs(int i, int sum, vector<int> v) {
	if(i == s.size()) {
		vmin = min(vmin, sum);
		vmax = max(vmax, sum);

		if(sum <= n) {
			if(ans < sum) {
				ans = sum;
				cnt = 1;
				res.clear();
				rep(i, v.size()) res.push_back(v[i]);
			} else if(ans == sum) {
				cnt++;
			}
		}
		return;
	}

	REP(j, 1, s.size() + 1) {
		if(i + j <= s.size() ) {
			vector<int> t(v.begin(), v.end());
			t.push_back(stoa(s.substr(i, j)));
			dfs(i + j, sum + stoa(s.substr(i, j)), t);
		} else return;
	}
}

int main() {
	while(cin >> n >> s) {
		if(n == 0 && s == "0") break;

		vmin = INF, vmax = 0, ans = 0, cnt = 0;
		res.clear();

		vector<int> v;
		dfs(0, 0, v);

		if(vmin > n) {
			cout << "error" << endl;
		} else if(cnt >= 2) {
			cout << "rejected" << endl; 
		} else {
			cout << ans;
			rep(i, res.size()) {
				cout << " " << res[i];
			}
			cout << endl;
		}
	}
	return 0;
}
```

