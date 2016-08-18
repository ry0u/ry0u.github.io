---
layout: post
title: "Codeforces354-div2D Theseus and labyrinth"
date: 2016-05-26 10:19:44 +0900
comments: true
categories: [Codeforces, 幅優先]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-branding="0" data-card-type="article"><h4><a href="http://codeforces.com/contest/676/problem/D">Problem - D - Codeforces</a></h4><p>Theseus has just arrived to Crete to fight Minotaur. He found a labyrinth that has a form of a rectangular field of size and consists of blocks of size 1 × 1. Each block of the labyrinth has a button that rotates all blocks 90 degrees clockwise.</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

サイズが {% m %} 1 \times 1 {% em %}のブロックで構成される {% m %} n \times m {% em %}の {% m %} field {% em %}がある． {% m %} 1 {% em %}回の行動で隣接するマスに移動するか，全てのブロックを {% m %} 90 {% em %}度時計回りに回転するかが出来る．隣接するマスには移動するためには，現在のマスから隣接するマスの方向にドアがあり，隣接するマスから現在のマスの方向にドアがあるのが条件となる． {% m %} (x_t, y_t) {% em %}から {% m %} (x_mm, y_m) {% em %}には最小何回でいけるか．  
  
盤面と状態を{% m %} (y, x, rot) {% em %}で持って，幅優先探索．当たられる座標が {% m %} xとy {% em %}で逆なので注意する．

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
typedef pair<int, P> IP;
typedef pair<P, P> PP;

int d[1001][1001][4];
char v[1001][1001][4];
map<char, vector<char> > ma;

// (y, x)にdirのドアがあるか
bool ch(int y, int x, int rot, int i) {
	char c = v[y][x][rot];
	if(c == '+') return true;
	if(c == '*') return false;

	if(i == 0) {
		if(c == '-' || c == '>' || c == 'D' || c == 'L' || c == 'U') return true;
		return false;
	} else if(i == 1) {
		if(c == '|' || c == 'v' || c == 'L' || c == 'U' || c == 'R') return true;
		return false;
	} else if(i == 2) {
		if(c == '-' || c == '<' || c == 'U' || c == 'R' || c == 'D') return true;
		return false;
	} else if(i == 3) {
		if(c == '|' || c == '^' || c == 'R' || c == 'D' || c == 'L') return true;
		return false;
	}
}

int main() {
	int n, m;
	cin >> n >> m;

	ma['+'].push_back('+');
	ma['+'].push_back('+');
	ma['+'].push_back('+');
	ma['+'].push_back('+');

	ma['-'].push_back('-');
	ma['-'].push_back('|');
	ma['-'].push_back('-');
	ma['-'].push_back('|');

	ma['|'].push_back('|');
	ma['|'].push_back('-');
	ma['|'].push_back('|');
	ma['|'].push_back('-');

	ma['^'].push_back('^');
	ma['^'].push_back('>');
	ma['^'].push_back('v');
	ma['^'].push_back('<');

	ma['>'].push_back('>');
	ma['>'].push_back('v');
	ma['>'].push_back('<');
	ma['>'].push_back('^');

	ma['v'].push_back('v');
	ma['v'].push_back('<');
	ma['v'].push_back('^');
	ma['v'].push_back('>');

	ma['<'].push_back('<');
	ma['<'].push_back('^');
	ma['<'].push_back('>');
	ma['<'].push_back('v');

	ma['L'].push_back('L');
	ma['L'].push_back('U');
	ma['L'].push_back('R');
	ma['L'].push_back('D');

	ma['U'].push_back('U');
	ma['U'].push_back('R');
	ma['U'].push_back('D');
	ma['U'].push_back('L');

	ma['R'].push_back('R');
	ma['R'].push_back('D');
	ma['R'].push_back('L');
	ma['R'].push_back('U');

	ma['D'].push_back('D');
	ma['D'].push_back('L');
	ma['D'].push_back('U');
	ma['D'].push_back('R');

	ma['*'].push_back('*');
	ma['*'].push_back('*');
	ma['*'].push_back('*');
	ma['*'].push_back('*');

	vector<string> s(n);
	rep(i, n) cin >> s[i];

	rep(i, n) {
		rep(j, m) {
			rep(k, 4) {
				v[i][j][k] = ma[s[i][j]][k];
			}
		}
	}

	int sy, sx;
	cin >> sy >> sx;

	sy--; sx--;

	int gy, gx;
	cin >> gy >> gx;

	gy--; gx--;

	rep(i, n) {
		rep(j, m) {
			rep(k, 4) d[i][j][k] = INF;
		}
	}

	queue<IP> que;
	que.push(mp(0, mp(sy, sx)));
	d[sy][sx][0] = 0;

	int dx[4] = {1, 0, -1, 0};
	int dy[4] = {0, 1, 0, -1};
	int nd[4] = {2, 3, 0, 1};

	while(que.size()) {
		IP p = que.front(); que.pop();
		int rot = p.first;
		int y = p.second.first;
		int x = p.second.second;

		rep(i, 4) {
			int ny = y + dy[i];
			int nx = x + dx[i];

			if(0 <= ny && ny < n && 0 <= nx && nx < m && ch(y, x, rot, i) && ch(ny, nx, rot, nd[i])) {
				if(d[ny][nx][rot] > d[y][x][rot] + 1) {
				   d[ny][nx][rot] = d[y][x][rot] + 1;
					que.push(mp(rot, mp(ny, nx)));
				}
			}
		}

		int nr = (rot + 1) % 4;
		if(d[y][x][nr] > d[y][x][rot] + 1) {
			d[y][x][nr] = d[y][x][rot] + 1;
			que.push(mp(nr, mp(y, x)));
		}
	}

	// rep(k, 4) {
	// 	cout  << "-------- " << endl;
	// 	rep(i, n) {
	// 		rep(j, m) {
	// 			if(d[i][j][k] == INF) cout << "X ";
	// 			else cout << d[i][j][k] << " ";
	// 		}
	// 		cout << endl;
	// 	}
	// }

	int ans = INF;
	rep(i, 4) {
		ans = min(ans, d[gy][gx][i]);
	}

	if(ans == INF) cout << -1 << endl;
	else cout << ans << endl;

	return 0;
}
```

実装がひどい．各方向に行けるかいけないかをboolで持っておけば， {% m %} ch {% em %}の中で全て列挙する形にならないので良いと思った．実装の方針がミスのしやすさ，実装時間に影響すると思うのでなるべく考えてから書きたい．
