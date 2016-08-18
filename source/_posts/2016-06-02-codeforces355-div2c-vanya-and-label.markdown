---
layout: post
title: "Codeforces355-div2C Vanya and Label"
date: 2016-06-02 14:36:16 +0900
comments: true
categories: [Codeforces, 組み合わせ]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-branding="0" data-card-type="article"><h4><a href="http://codeforces.com/contest/677/problem/C">Problem - C - Codeforces</a></h4><p>While walking down the street Vanya saw a label "Hide&Seek". Because he is a programmer, he used & as a bitwise AND for these two words represented as a integers in base 64 and got new word.</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

文字列$s$と$\\&$を取った時に等しくなる$2$つの文字列の組み合わせいくつあるか？ $mod\ 10 ^9 + 7$で求める．  
  
各桁についての組み合わせを出して掛けていった．組み合わせを全探索して組み合わせ数を出した． $\\&と==$では$==$のほうが優先度が高いのでちゃんと括弧でくくる．

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
#include <bitset>

#define REP(i,k,n) for(int i=k;i<n;i++)
#define rep(i,n) for(int i=0;i<n;i++)
#define INF 1<<30
#define pb push_back
#define mp make_pair
#define MOD 1000000007

using namespace std;
typedef long long ll;
typedef pair<int,int> P;

int main() {
	vector<char> v;
	map<char, int> m;
	rep(i, 10) {
		m['0'+i] = i;
		v.push_back('0'+i);
	}

	rep(i, 26) {
		m['A'+i] = 10 + i;
		m['a'+i] = 36 + i;

		v.push_back('A'+i);
		v.push_back('a'+i);
	}

	m['-'] = 62;
	m['_'] = 63;
	v.push_back('-');
	v.push_back('_');

	map<char, ll> cnt;
	sort(v.begin(), v.end());

	rep(i, v.size()) {
		rep(j, v.size()) {
			rep(k, v.size()) {
				if((m[v[j]] & m[v[k]]) == m[v[i]]) {
					cnt[v[i]]++;
				}
			}
		}
	}

	ll ans = 1;
	string s;
	cin >> s;

	rep(i, s.size()) {
		ans *= cnt[s[i]];
		ans %= MOD;
	}

	cout << ans << endl;

	return 0;
}
```

