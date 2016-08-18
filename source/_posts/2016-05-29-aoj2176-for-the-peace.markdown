---
layout: post
title: "AOJ2176 For the Peace"
date: 2016-05-29 23:33:39 +0900
comments: true
categories: [AOJ-ICPC, 350]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-branding="0" data-card-type="article-full"><h4><a href="http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=2176">For the Peace</a></h4><p>This is a story of a world somewhere far from the earth. In this world, the land is parted into a number of countries ruled by empires. This world is not very peaceful: they have been involved in army race. They are competing in production of missiles in particular.</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

それぞれの国の {% m %} war\ potential {% em %}はミサイルの威力の合計で決まる．ミサイルはそれぞれの国と差が {% m %} d {% em %}以下なら捨てることが出来る．全ての国はミサイルを捨てることが出来るか？  
  
何かを捨てたことによって，本来捨てられるべきものが捨てられなかった，という場合がないので，捨てられるものから捨てていった．


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
#include <stack>

#define REP(i,k,n) for(int i=k;i<n;i++)
#define rep(i,n) for(int i=0;i<n;i++)
#define INF 1<<30
#define pb push_back
#define mp make_pair

using namespace std;
typedef long long ll;
typedef pair<int,int> P;

int main() {
	int n, d;
	while(cin >> n >> d) {
		if(n == 0 && d == 0) break;

		stack<int> st[n];
		vector<int> sum(n);
		rep(i, n) {
			int m;
			cin >> m;

			rep(j, m) {
				int x;
				cin >> x;

				st[i].push(x);
				sum[i] += x;
			}
		}

		int id = 0, cnt = 0;
		while(cnt != n) {
			if(st[id].size() == 0) {
				id = (id + 1) % n;
				cnt++;
				continue;
			}

			int p = st[id].top();
			sum[id] -= p;

			int vmin = INF, vmax = 0;
			rep(i, n) {
				vmin = min(vmin, sum[i]);
				vmax = max(vmax, sum[i]);
			}

			if(vmax - vmin <= d) {
				st[id].pop();
				cnt = 0;
			} else {
				sum[id] += p;
				cnt++;
			}

			id = (id + 1) % n;
		}

		bool flag = true;
		rep(i, n) {
			if(sum[i] == 0) continue;
			flag  = false;
		}

		if(flag) cout << "Yes" << endl;
		else cout << "No" << endl;
	}
	return 0;
}
```
