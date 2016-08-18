---
layout: post
title: "AOJ2002 X-Ray Screening System"
date: 2016-03-23 19:25:35 +0900
comments: true
categories: [AOJ-ICPC, 300, 実装]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-type="article" data-card-branding="0"><h4><a href="http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=2002">X-Ray Screening System | Aizu Online Judge</a></h4><p>上記の調査結果をふまえて，以下のような手荷物検査のためのモデルを考案した．それぞれの手荷物は X 線に対して透明である直方体の容器だとみなす．その中には X 線に対して不透明である複数の品物が入っている．ここで，直方体の 3 辺を x 軸，y 軸，z 軸とする座標系を考え，x 軸と平行な方向に X 線を照射して，y-z 平面に投影された画像を撮影する．撮影された画像は適当な大きさの格子に分割され，画像解析によって，それぞれの格子領域に映っている品物の材質が推定される．この会社には非常に高度の解析技術があり，材質の詳細な違いすらも解析することが可能であることから，品物の材質は互いに異なると考えることができる．なお，複数の品物が x 軸方向に関して重なっているときは，それぞれの格子領域について最も手前にある，すなわち x 座標が最小である品物の材質が得られる．また，2 つ以上の品物の x 座標が等しいことはないと仮定する．</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

もっとも手前にある品物から順番に処理していく．長方形なので，一番左上{% m %} (sx, sy) {% em %}と右下 {% m %} (gx, gy) {% em %}を持っておき， {% m %} (sx, sy) {% em %}， {% m %} (gx, sy) {% em %}， {% m %} (sx, gy) {% em %}， {% m %} (gx, gy) {% em %}の4角形全てに同じ品物の材質，または前面に他の品物がある時に埋めた．

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

int w, h;
int sy, sx, gy, gx;
int dx[4] = {1,0,-1,0};
int dy[4] = {0,1,0,-1};

bool can(int y,int x) {
	if(0 <= y && y < h && 0 <= x && x < w) return true;
	return false;
}

vector<string> s;
char c;
bool used[55][55], visited[55][55];

bool check() {
	REP(i, sy, gy+1){
		REP(j, sx, gx+1) {
			if(s[i][j] == c || used[i][j]) continue;
			return false;
		}
	}
	return true;
}

void f() {
	REP(i, sy, gy+1) {
		REP(j, sx, gx+1) {
			used[i][j] = true;
		}
	}
}

int main() {
	int n;
	cin >> n;

	rep(q, n) {
		cin >> h >> w;
		s.resize(h);
		rep(i, h) cin >> s[i];

		memset(used, 0, sizeof(used));
		set<char> S;

		bool flag = true, update = true;
		while(update) {
			update = false;
			rep(i, h) {
				rep(j, w) {
					if(s[i][j] == '.') continue;

					if(used[i][j]) continue;

					sy = i;
					sx = j;
					gy = i;
					gx = j;
					c = s[i][j];
					memset(visited, 0, sizeof(visited));

					rep(k, h) {
						rep(l, w) {
							if(s[k][l] == c) {
								sy = min(sy, k);
								sx = min(sx, l);
								gy = max(gy, k);
								gx = max(gx, l);
							}
						}
					}

					if(S.find(c) == S.end() && check()) {
						f();
						S.insert(c);
						update = true;
					}
				}
			}
		}

		rep(i, h) {
			rep(j, w) {
				if(used[i][j]) continue;
				if(s[i][j] == '.') continue;
				flag = false;
			}
		}

		if(flag) cout << "SAFE" << endl;
		else cout << "SUSPICIOUS" << endl;

	}

	return 0;
}
```

