---
layout: post
title: "AOJ1188 Hierarchical Democracy"
date: 2016-03-23 18:25:06 +0900
comments: true
categories: [AOJ-ICPC, 300, 深さ優先, 実装]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-type="article" data-card-branding="0"><h4><a href="http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=1188">Hierarchical Democracy</a></h4><p>The presidential election in Republic of Democratia is carried out through multiple stages as follows. There are exactly two presidential candidates. At the first stage, eligible voters go to the polls of his/her electoral district. The winner of the district is the candidate who takes a majority of the votes.</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

木として考える．葉にその区画に勝つための最小の値， {% m %} \frac{値}{2} + 1 {% em %}を入れる．子の数の半分，小さい順に取っていく．最後に根の値が最小値になっているはず．  

{% img /images/AOJ/1188-1.png %}
{% img /images/AOJ/1188-2.png %}
  
{% img /images/AOJ/1188-3.png %}
  
子の階層から小さい順に取りたいので，priority_queueを利用した．

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
#include <queue>

#define REP(i,k,n) for(int i=k;i<n;i++)
#define rep(i,n) for(int i=0;i<n;i++)
#define INF 1<<30
#define pb push_back
#define mp make_pair

using namespace std;
typedef long long ll;
typedef pair<int,int> P;

priority_queue<ll, vector<ll>, greater<ll> > que[100005];

int main() {
	int n;
	cin >> n;

	rep(q, n) {
		rep(i, 100005) {
			while(que[i].size()) que[i].pop();
		}

		string s;
		cin >> s;

		int dep = 0;
		rep(i, s.size()) {

			if(s[i] == '[') {
				if(s[i+1] == '[') {
					dep++;
				} else {
					stringstream ss;
					REP(j, i+1, s.size()) {
						if('0' <= s[j] && s[j] <= '9') {
							ss << s[j];
							i++;
						} else {
							i++;
							break;
						}
					}

					ll x;
					ss >> x;

					x = x / 2 + 1;
					que[dep].push(x);
				}
			} else if(s[i] == ']') {
				int m = que[dep].size();
				ll sum = 0;

				rep(j, m/2 + 1) {
					sum += que[dep].top();
					que[dep].pop();
				}

				que[dep-1].push(sum);

				while(que[dep].size()) {
					que[dep].pop();
				}

				dep--;
			}

		}

		cout << que[0].top() << endl;
	}

	return 0;
}
```

