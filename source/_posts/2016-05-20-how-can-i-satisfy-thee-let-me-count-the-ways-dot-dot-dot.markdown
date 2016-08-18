---
layout: post
title: "AOJ1155 How can I satisfy thee? Let me count the ways..."
date: 2016-05-20 22:38:10 +0900
comments: true
categories: [AOJ-ICPC, 350, 構文解析]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-branding="0" data-card-image="http://judge.u-aizu.ac.jp/onlinejudge/IMAGE1/2008C2.png" data-card-type="article"><h4><a href="http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=1155&lang=jp">How can I satisfy thee? Let me count the ways... | Aizu Online Judge</a></h4><p>Introduction to Programming Introduction to Algorithms and Data Structures Library of Data Structures Library of Graph Algorithms Library of Computational Geometry Library of Dynamic Programming Library of Number Theory</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

構文解析．
{% math %}
\begin{eqnarray}
<formula>\ &::=&\ 0\ or\ 1\ or\ 2 \\
<formula>\ &::=&\ -<formula> \\
<formula>\ &::=&\ (<formula>\ +\ or\ * <formula>) \\
\end{eqnarray}
{% endmath %}
の場合に分けて考える．入れ子の括弧にどう対応させるかよく分からなかったが，自分なりに理解することができた．構文解析の関数が出来たら，{% m %} (P, Q, R) {% em %}の組み合わせ({% m %} 2 ^3 {% em %}通り)を全列挙して， {% m %} 2 {% em %}となる個数を数える．

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

int formula(string& s, int& i) {
	if(s[i] == '(') {
		i++;
		int val = formula(s, i);

		char op = s[i];

		i++;
		int val2 = formula(s, i);

		int ret = 0;
		if(op == '+') {
			if(val == 2 || val2 == 2) ret = 2;
			else if(val == 1 || val2 == 1) ret = 1;
			else ret = 0;
		}
		if(op == '*') {
			if(val == 2 && val2 == 2) ret = 2;
			else if((val == 1 || val == 2) && (val2 == 1 || val2 == 2)) ret = 1;
			else ret = 0;
		}

		i++;
		return ret;
	} else if(isdigit(s[i])) {
		int ret = (s[i] - '0');
		i++;
		return ret;
	} else if(s[i] == '-') {
		i++;
		int val = formula(s, i);

		int ret = 0;
		if(val == 0) ret = 2;
		if(val == 1) ret = 1;
		if(val == 2) ret = 0;

		return ret;
	}
}


int main() {
	string s;
	while(cin >> s) {
		if(s == ".") break;

		int ans = 0;
		rep(i, 3) {
			rep(j, 3) {
				rep(k, 3) {
					string t = s;
					rep(l, s.size()) {
						if(t[l] == 'P') t[l] = ('0' + i);
						else if(t[l] == 'Q') t[l] = ('0' + j);
						else if(t[l] == 'R') t[l] = ('0' + k);
					}

					int p = 0;
					if(formula(t, p) == 2) {
						ans++;
					}
				}
			}
		}

		cout << ans << endl;
	}

	return 0;
}
```

