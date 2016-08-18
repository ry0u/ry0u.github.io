---
layout: post
title: "Codeforces334-div2C Alternative Thinking"
date: 2015-12-15 09:50:00 +0900
comments: true
categories: [Codeforces]
---

長さ{% m %}N{% em %}の'0'と'1'で構成された文字列が与えられる．ある区間を1度だけ反転して'0'と'1'が交互となる列の長さを最大化する．

# 考察
まず，文字列の交互列の長さを{% m %} m {% em %}とすると，一度の反転で最大2しか増えないことが分かる．例えば，
{% math %}
	1111(m = 1) \Rightarrow 11\color{red}01(m = 3) \\
	1101(m = 3) \Rightarrow \color{red}0101(m = 4) \\
	1100(m = 2) \Rightarrow 1\color{red}0\color{red}10(m = 4) \\
{% endmath %}
のように出来る．また，{% m %} m = N {% em %}の時以外に，区間を反転して長さが増えないケースを考えると，そのようなケースは無いと分かる．反転する区間で01を内包している場合，反転後をその部分は10で交互列になるからである．
{% math %}
11 0101 00(m = 4) \Rightarrow 1 \color{red}0 \color{red}1\color{red}0\color{red}1\color{red}0 \color{red}1 0(m = 6) \\
{% endmath %}

よって，{% m %}N{% em %}を超えないように，+1，+2したらACが貰えた

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

	string s;
	cin >> s;

	int cnt = 0;
	bool flag = true;

	if(s[0] == '1') cnt++;

	rep(i, n) {
		if(flag) {
			if(s[i] == '1') continue;
			else {
				cnt++;
				flag = false;
			}
		} else {
			if(s[i] == '1') {
				cnt++;
				flag = true;
			} else continue;
		}
	}

	if(cnt + 2 <= n) cout << cnt + 2 << endl;
	else if(cnt + 1 <= n) cout << cnt + 1 << endl;
	else cout << cnt << endl;

	return 0;
}
```
