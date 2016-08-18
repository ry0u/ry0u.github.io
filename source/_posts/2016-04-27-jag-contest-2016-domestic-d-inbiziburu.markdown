---
layout: post
title: "JAG Contest 2016 Domestic D インビジブル"
date: 2016-04-27 23:39:09 +0900
comments: true
categories: [模擬予選, メモ化再帰]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-type="article"><h4><a href="http://jag2016-domestic.contest.atcoder.jp/tasks/jag2016secretspring_d">D: インビジブル - JAG Contest 2016 Domestic | AtCoder</a></h4><p>(null)</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

状態を({% m %} a {% em %}のデッキから何枚取ったか， {% m %} b {% em %}のデッキから何枚取ったか，場のスタック，何ターン目，前回のスタックが空の状態でパスをしたかどうか)にしてメモ化再帰．minimaxみたいな感じでプレイヤー {% m %} 1 {% em %}のターンでは {% m %} c - d {% em %}の最大化，プレイヤー {% m %} 2 {% em %}のターンでは {% m %} c - d {% em %}の最小化をした．  

本番では，メモ化するときに，stackの状態を持たねばならないと思っていたが，スタックのサイズだけを持てば良いことに気付けなかった．スタックが空の状態で無くてもパスパスをした時点で終了だと思い込んでいた．

# Code

```cpp
#include <iostream>
#include <sstream>
#include <string>
#include <cstring>
#include <vector>
#include <algorithm>
#include <map>
#include <stack>

#define REP(i, k, n) for(int i = k; i < n; i++) 
#define rep(i, n) for(int i = 0; i < n; i++) 
#define INF 1<<30

using namespace std;
typedef pair<int, int> P;

int n, m;
vector<int> a, b;

int f(stack<P> &st) {
	int c = 0, d = 0;
	bool cf = true, df = true;
	while(st.size()) {
		P p = st.top(); st.pop();
		if(p.first == 0) {
			if(p.second == -1) {
				df = false;
			} else if(cf) {
				c += p.second;
			}
		} else {
			if(p.second == -1) {
				cf = false;
			} else if(df) {
				d += p.second;
			}
		}
	}
	return c - d;
}

int memo[55][55][2][2][105];

int dfs(int i, int j, stack<P> st, int turn, bool flag) {
	if(memo[i][j][turn % 2][flag][st.size()] != INF) return memo[i][j][turn % 2][flag][st.size()];

	int ret = 0, size = st.size();
	if(flag) {
		if(turn % 2 == 0) {
			ret = 0;
			if(i != n) {
				st.push(P(0, a[i]));
				ret = max(ret, dfs(i + 1, j, st, turn + 1, false));
				st.pop();
			}
		} else {
			ret = 0;
			if(j != m) {
				st.push(P(1, b[j]));
				ret = min(ret, dfs(i, j + 1, st, turn + 1, false));
				st.pop();
			}
		}
	} else {
		if(turn % 2 == 0) {
			ret = -INF;
			if(i != n) {
				st.push(P(0, a[i]));
				ret = max(ret, dfs(i + 1, j, st, turn + 1, false));
				st.pop();
			}

			bool ch = (st.size() == 0);
			int d = f(st);
			ret = max(ret, dfs(i, j, st, turn + 1, ch) + d);
		} else {
			ret = INF;

			if(j != m) {
				st.push(P(1, b[j]));
				ret = min(ret, dfs(i, j + 1, st, turn + 1, false));
				st.pop();
			}

			bool ch = (st.size() == 0);
			int d = f(st);
			ret = min(ret, dfs(i, j, st, turn + 1, ch) + d);
		}
	}

	return memo[i][j][turn % 2][flag][size] = ret;
}

int main() {
	cin >> n >> m;

	a.resize(n);
	rep(i, n) cin >> a[i];

	b.resize(m);
	rep(i, m) cin >> b[i];

	stack<P> st;
	rep(i, 55) rep(j, 55) rep(k, 2) rep(l, 2) rep(o, 105) memo[i][j][k][l][o] = INF;
	cout << dfs(0, 0, st, 0, false) << endl;
	return 0;
}

```

