---
layout: post
title: "AOJ0612 Sandcastle"
date: 2016-03-18 15:40:40 +0900
comments: true
categories: [AOJ, シュミレーション]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-type="article" data-card-branding="0"><h4><a href="http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=0612">Sandcastle | Aizu Online Judge</a></h4><p>Introduction to Programming Introduction to Algorithms and Data Structures Library of Data Structures Library of Graph Algorithms Library of Computational Geometry Library of Dynamic Programming Library of Number Theory</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

最初に崩壊するマスをqueueに突っ込む．そのマスが崩壊したことによって崩壊するマスは隣接しているマスのみなので，queueから取ってきた時に隣接するマスのカウンタを{% m %} +1 {% em %}して崩壊するならまたqueueに突っ込む．

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

int h, w;
int dy[8] = { 1, 1, 1, 0, 0,-1,-1,-1};
int dx[8] = { 1, 0,-1, 1,-1, 1, 0,-1};

bool can(int y, int x) {
	if(0 <= y && y < h && 0 <= x && x < w) return true;
	return false;
}


int cnt[1005][1005];
bool inQ[1005][1005], used[1005][1005];

int main() {
	cin >> h >> w;

	vector<string> s(h);
	rep(i, h) cin >> s[i];

	memset(cnt, 0, sizeof(cnt));
	memset(used, 0, sizeof(used));
	memset(inQ, 0, sizeof(inQ));

	queue<P> que;
	rep(i, h) {
		rep(j, w) {
			if(s[i][j] != '.') {
				rep(k, 8) {
					int y = i + dy[k];
					int x = j + dx[k];
					if(can(y, x) && s[y][x] == '.') {
						cnt[i][j]++;
					}
				}
				if(s[i][j] - '0' <= cnt[i][j]) {
					que.push(mp(i, j));
					used[i][j] = true;
				}
			}
		}
	}


	int ans = 0;
	while(que.size()) {
		queue<P> tmp;
		while(que.size()) {
			P p = que.front();
			que.pop();

			rep(k, 8) {
				int y = p.first + dy[k];
				int x = p.second + dx[k];

				if(can(y, x)) {
					cnt[y][x]++;
					if(s[y][x] != '.' && (s[y][x] - '0') <= cnt[y][x] && !used[y][x]) {
						used[y][x] = true;
						tmp.push(mp(y, x));
					}
				}
			}
		}

		while(tmp.size()) {
			P p = tmp.front();
			tmp.pop();
			que.push(p);
		}

		ans++;
	}

	cout << ans << endl;

	return 0;
}
```

