---
layout: post
title: "Codeforces354-div2B Pyramid of Glasses"
date: 2016-05-26 09:11:14 +0900
comments: true
categories: [codeforces, シュミレーション]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-branding="0" data-card-type="article"><h4><a href="http://codeforces.com/contest/676/problem/B">Problem - B - Codeforces</a></h4><p>Mary has just graduated from one well-known University and is now attending celebration party. Students like to dream of a beautiful life, so they used champagne glasses to construct a small pyramid. The height of the pyramid is n.</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

グラスがピラミッドのように並んでいる．上から{% m %} n {% em %}段目には{% m %} n {% em %}個のグラスがある．そのグラスがいっぱいになった時は{% m %} 1 {% em %}段下のグラスに均等に注がれる．{% m %} t {% em %}秒後にいっぱいになっているグラスはいくつあるか？  
  

実際にグラフを作って，グラス{% m %} 0 {% em %}から {% m %} 1 {% em %}秒ずつ流していく．グラフは， {% m %} i {% em %}段目の頂点は(自分の番号 {% m %} + i {% em %}), (自分の番号{% m %} + i + 1 {% em %})と繋がるようにした．  

{% img /images/Codeforces/354/b.png %}

流す量を {% m %} 1.0 {% em %}から初めて，いっぱいになっている場合は，その半分を繋がっている頂点に流す．流れる量は必ず {% m %} \frac{1}{2 ^x} {% em %}という形になり， {% m %} n {% em %}は最大で {% m %} 10 {% em %}段なので誤差無く保持出来る(はず)．


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

vector<int> g[105];
double v[105];

void dfs(int cur, double x) {
	if(v[cur] == 1.0) {
		rep(i, g[cur].size()) {
			dfs(g[cur][i], x / 2.0);
		}
	} else {
		v[cur] += x;
		return;
	}
}

int main() {
	int n, t;
	cin >> n >> t;

	int m = (n * (n+1) ) / 2;

	memset(v, 0, sizeof(v));
	int id = 0, len = 1;
	rep(i, n-1) {
		rep(j, i+1) {
			g[id].push_back(id + len);
			g[id].push_back(id + len+1);
			id++;
		}
		len++;
	}

	rep(i, t) {
		dfs(0, 1.0);
	}

	int cnt = 0;
	rep(i, m) {
		if(v[i] >= 1.0) cnt++;
	}

	cout << cnt << endl;

	return 0;
}
```

