---
layout: post
title: "AOJ1316 The Sorcerer's Donut"
date: 2016-03-20 22:29:14 +0900
comments: true
categories: [AOJ-ICPC, 250, 実装]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-type="article" data-card-branding="0"><h4><a href="http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=1316">The Sorcerer's Donut</a></h4><p>Your master went to the town for a day. You could have a relaxed day without hearing his scolding. But he ordered you to make donuts dough by the evening. Loving donuts so much, he can't live without eating tens of donuts everyday. What a chore for such a beautiful day.</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

盤面が小さいので，ある方向に一周した文字列を列挙して {% m %} 2 {% em %}回以上出て辞書順最小のものを出力した．

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

int w,h,x,y;
int sx,sy,gx,gy;
int dx[9] = { 1, 1, 1, 0, 0, 0,-1,-1,-1};
int dy[9] = {-1, 0, 1,-1, 0, 1,-1, 0, 1};

bool can(int y,int x) {
	if(0 <= y && y < h && 0 <= x && x < w) return true;
	return false;
}

int main() {
	while(cin >> h >> w) {
		if(h == 0 && w == 0) break;

		vector<string> s(h);
		rep(i, h) cin >> s[i];

		map<string, int> m;
		bool used[15][25];

		rep(i, h) {
			rep(j, w) {
				rep(k, 9) {
					int y = i, x = j;
					memset(used, 0, sizeof(used));
					string t = "";
					while(!used[y][x]) {
						used[y][x] = true;
						t += s[y][x];

						y = (y + h + dy[k]) % h;
						x = (x + w + dx[k]) % w;

						if(t.size() > 1) m[t]++;
					}
				}
			}
		}

		string ans = "";
		map<string, int>::iterator ite;
		for(ite = m.begin(); ite != m.end(); ite++) {
			if(ite->second < 2) continue;
			if(ite->first.size() > ans.size()) {
				ans = ite->first;
			} else if(ite->first.size() == ans.size()) {
				bool flag = true;
				rep(i, ans.size()) {
					if(ite->first[i] <= ans[i]) continue;
					flag = false;
				}

				if(flag) ans = ite->first;
			}
		}

		if(ans.size() == 0) cout << 0 << endl;
		else cout << ans << endl;
	}

	return 0;
}
```

