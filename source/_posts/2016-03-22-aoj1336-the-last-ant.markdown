---
layout: post
title: "AOJ1336 The Last Ant"
date: 2016-03-22 20:09:32 +0900
comments: true
categories: [AOJ-ICPC, 250]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-type="article" data-card-branding="0"><h4><a href="http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=1336">The Last Ant</a></h4><p>A straight tunnel without branches is crowded with busy ants coming and going. Some ants walk left to right and others right to left. All ants walk at a constant speed of 1 cm/s. When two ants meet, they try to pass each other.</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

それぞれ左右に動き，他の蟻にぶつかった時に向きを反転する蟻が {% m %} N {% em %}匹いる．最後に巣を出た蟻の番号を答える．  
左右の移動は確定で，移動した後に同じ場所に蟻が {% m %} 2 {% em %}匹いれば向きを反転する．どうタイミングで巣を出た時は左側から出たの蟻の番号を答えるのを忘れない．

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

int main() {
	int n, l;
	while(cin >> n >> l) {
		if(n == 0 && l == 0) break;

		vector<char> dir(n);
		vector<int> p(n);
		rep(i, n) {
			cin >> dir[i] >> p[i];
		}

		int cnt = 0, id = -1, next[105];
		bool used[25];
		memset(used, 0, sizeof(used));

		while(true) {
			memset(next, 0, sizeof(next));

			rep(i, n) {
				if(used[i]) continue;
				if(dir[i] == 'R') p[i]++;
				else p[i]--;

				next[p[i]]++;
			}

			rep(i, n) {
				if(used[i]) continue;
				if(next[p[i]] == 2) {
					dir[i] = (dir[i] == 'R' ? 'L' : 'R');
				}
			}

			rep(i, n) {
				if(used[i]) continue;
				if(p[i] == l) {
					used[i] = true;
					id = i;
				}
			}

			rep(i, n) {
				if(used[i]) continue;
				if(p[i] == 0) {
					used[i] = true;
					id = i;
				}
			}

			bool flag = true;
			rep(i, n) {
				if(used[i]) continue;
				flag = false;
			}

			if(flag) break;
			cnt++;
		}

		cout << cnt + 1 << " " << id + 1 << endl;
	}

	return 0;
}
```

