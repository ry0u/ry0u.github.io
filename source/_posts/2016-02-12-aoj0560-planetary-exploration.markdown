---
layout: post
title: "AOJ0560 Planetary Exploration"
date: 2016-02-12 02:11:59 +0900
comments: true
categories: [AOJ, いもす法]
---

問題文  
http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=0560

<!-- more -->

2次元imos．{% m %} (a, b) 〜 (c, d) {% em %}は

1. f\[a-1]\[b-1]
2. f\[a-1]\[d]
3. f\[c]\[b-1] 
4. f\[c]\[d]

1
{% img /images/AOJ/0560-4.png %}
2
{% img /images/AOJ/0560-3.png %}  
3
{% img /images/AOJ/0560-2.png %}
4
{% img /images/AOJ/0560-1.png %}

とすると，{% m %} f(c, d) - f(c, b-1) - f(a-1, d) + f(a-1, b-1) {% em %}で求めることが出来る．

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

int J[1005][1005], O[1005][1005], I[1005][1005];

int main() {
	int m, n, k;
	cin >> m >> n >> k;

	vector<string> v(m);
	rep(i, m) cin >> v[i];

	memset(J, 0, sizeof(J));
	memset(O, 0, sizeof(O));
	memset(I, 0, sizeof(I));

	rep(i, m) {
		rep(j, n) {
			if(v[i][j] == 'J') J[i][j]++;
			if(v[i][j] == 'O') O[i][j]++;
			if(v[i][j] == 'I') I[i][j]++;
		}
	}

	rep(i, m) {
		REP(j, 1, n) {
			J[i][j] += J[i][j-1];
			O[i][j] += O[i][j-1];
			I[i][j] += I[i][j-1];
		}
	}

	rep(i, n) {
		REP(j, 1, m) {
			J[j][i] += J[j-1][i];
			O[j][i] += O[j-1][i];
			I[j][i] += I[j-1][i];
		}
	}

	rep(i, k) {
		int a, b, c, d;
		cin >> a >> b >> c >> d;

		a--;
		b--;
		c--;
		d--;

		cout << J[c][d] - (a-1 >= 0 ? J[a-1][d] : 0) - (b-1 >= 0 ? J[c][b-1] : 0) + (a-1 >= 0 && b -1 >= 0 ? J[a-1][b-1] : 0) << " ";
		cout << O[c][d] - (a-1 >= 0 ? O[a-1][d] : 0) - (b-1 >= 0 ? O[c][b-1] : 0) + (a-1 >= 0 && b -1 >= 0 ? O[a-1][b-1] : 0) << " ";
		cout << I[c][d] - (a-1 >= 0 ? I[a-1][d] : 0) - (b-1 >= 0 ? I[c][b-1] : 0) + (a-1 >= 0 && b -1 >= 0 ? I[a-1][b-1] : 0) << endl;
	}

	return 0;
}
```

ベースを{% m %} (0, 0)にするとa == 0 || b == 0の時にちょっと面倒 {% em %}．
