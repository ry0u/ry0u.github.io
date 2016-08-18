---
layout: post
title: "AOJ2340 Carpenter's Language"
date: 2016-03-22 16:17:27 +0900
comments: true
categories: [AOJ-ICPC, 250]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-type="article" data-card-branding="0"><h4><a href="http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=2340">Carpenters' Language</a></h4><p>International Carpenters Professionals Company (ICPC) is a top construction company with a lot of expert carpenters. What makes ICPC a top company is their original language. The syntax of the language is simply given in CFG as follows: S -> SS | (S) | )S( | ε In other words, a right parenthesis can be closed by a left parenthesis and a left parenthesis can be closed by a right parenthesis in this language.</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

ICPC's languageは以下の法則で作られる．

{% math %}
	S \to \ \ SS \ \ | \ \ (S) \ \ | \ \ )S( \ \ | \ \ \varepsilon  \\
{% endmath %}

どんな{% m %} \(, \) {% em %}で構成される文字列も {% m %} \(と\) {% em %}の数が合っていれば構成出来るので， {% m %} \(を+ {% em %}， {% m %} \)を- {% em %}とし合計が {% m %} 0 {% em %}になる場合にYES，そうでない場合にNOを出力した．

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

	ll sum = 0;
	rep(i, n) {
		int a, b;
		char c;

		cin >> a >> c >> b;

		if(c == '(') sum += b;
		else sum -= b;

		if(sum == 0) cout << "Yes" << endl;
		else cout << "No" << endl;
	}

	return 0;
}
```

