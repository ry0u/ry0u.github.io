---
layout: post
title: "AOJ0604 Take the 'IOI' train"
date: 2016-03-16 00:31:03 +0900
comments: true
categories: [AOJ, 動的計画法]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-type="article" data-card-branding="0"><h4><a href="http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=0604">Take the 'IOI' train | Aizu Online Judge</a></h4><p>Introduction to Programming Introduction to Algorithms and Data Structures Library of Data Structures Library of Graph Algorithms Library of Computational Geometry Library of Dynamic Programming Library of Number Theory</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

待機用レールは列車を組み始めたら使えない．最初に避難させることは出来るが使いはじめる場所を決めたら，その列車を使うか，その列車以降を使わないかになる．

{% math %}
\begin{eqnarray}
	dp[i][j][0] &:=& Sをiまで，Tをjまで見て，末尾がOの時の最大値 \\
	dp[i][j][1] &:=& Sをiまで，Tをjまで見て，末尾がIの時の最大値
\end{eqnarray}
{% endmath %}

このように状態を持つと遷移は4パターンになる．  

{% img /images/AOJ/0604-1.png %}
{% img /images/AOJ/0604-2.png %}
  
{% img /images/AOJ/0604-3.png %}
{% img /images/AOJ/0604-4.png %}
  
最初は {% m %} I {% em %}で始め， {% m %} I {% em %}で終わることに注意する．また，どちらかの列車を全く使わなくても良いので最初に空白を入れて表現してみた(変だ)．


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

int dp[2005][2005][2];

int main() {
	int n, m;
	cin >> n >> m;

	string s, t;
	cin >> s >> t;

	n++;
	m++;
	s = " " + s;
	t = " " + t;

	memset(dp, 0, sizeof(dp));

	REP(i, 0, n) {
		REP(j, 0, m) {

			if(i == 0 && j == 0) continue;

			bool a = false, b = false;
			if(i == 0) {
				b = true;
			} else if(j == 0) {
				a = true;
			} else {
				a = true;
				b = true;
			}

			if(a) {
				if(dp[i-1][j][1] == 0) {
					if(s[i] == 'I') {
						dp[i][j][1] = max(dp[i][j][1], dp[i-1][j][0] + 1);
					}
				} else {
					if(s[i] == 'I') {
						dp[i][j][1] = max(dp[i][j][1], dp[i-1][j][0] + 1);
					} else {
						dp[i][j][0] = max(dp[i][j][0], dp[i-1][j][1] + 1);
					}
				}
			}

			if(b) {
				if(dp[i][j-1][1] == 0) {
					if(t[j] == 'I') {
						dp[i][j][1] = max(dp[i][j][1], dp[i][j-1][0] + 1);
					}
				} else {
					if(t[j] == 'I') {
						dp[i][j][1] = max(dp[i][j][1], dp[i][j-1][0] + 1);
					} else {
						dp[i][j][0] = max(dp[i][j][0], dp[i][j-1][1] + 1);
					}
				}
			}
		}
	}

	int ans = 0;
	rep(i, n) {
		rep(j, m) {
			// cout << "(" << dp[i][j][0] << "," << dp[i][j][1] << ") ";
			ans = max(ans, dp[i][j][1]);
		}
		// cout << endl;
	}

	cout << ans << endl;

	return 0;
}
```

