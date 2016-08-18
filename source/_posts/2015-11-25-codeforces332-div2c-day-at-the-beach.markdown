---
layout: post
title: "Codeforces332-div2C Day at the Beach"
date: 2015-11-25 01:38:53 +0900
comments: true
categories: [codeforces]
---

数列 h があり，hをソートした状態にしたい．区間に分けるとその区間ではソートすることが出来る．この分ける区間の数を最大化したい．

# 考察
数列hをソートし，ソート前とソート後で比較する．左から見て個数をカウントしていき，それが0に担った所を区切る．この区切り方は，数列hの区間をソートした数列が，ソートした数列hと一緒になる一番短い区間になる方法である．よってこれを繰り返すことで最大値が求まる．

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

	vector<int> v(n), t(n);
	rep(i, n) {
		cin >> v[i];
		t[i] = v[i];
	}

	sort(t.begin(), t.end());
	int ans = 0;
	map<int, int> m;

	rep(i, n) {
		if(m[v[i]] == -1) {
			m.erase(v[i]);
		} else {
			m[v[i]]++;
		}

		if(m[t[i]] == 1) {
			m.erase(t[i]);
		} else {
			m[t[i]]--;
		}

		if(m.size() == 0) {
			ans++;
		}
	}

	cout << ans << endl;

	return 0;
}
```

本番中はよくわからないコードを書いていた．終了後にソートしたものと比較すれば良いことに気付いた．．．
