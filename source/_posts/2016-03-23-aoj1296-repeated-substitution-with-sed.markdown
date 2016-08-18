---
layout: post
title: "AOJ1296 Repeated Substitution with Sed"
date: 2016-03-23 20:48:42 +0900
comments: true
categories: [AOJ-ICPC, 300, 幅優先]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-type="article" data-card-branding="0"><h4><a href="http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=1296">Repeated Substitution with Sed</a></h4><p>Do you know "sed," a tool provided with Unix? Its most popular use is to substitute every occurrence of a string contained in the input string (actually each input line) with another string β. More precisely, it proceeds as follows. Within the input string, every non-overlapping (but possibly adjacent) occurrences of α are marked.</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

文字列から {% m %} \alpha_i {% em %}の文字列を探して置換を繰り返す．探すのはstring::find, 置換はstring::replaceを使用して楽をした．幅優先で文字列が {% m %} \delta {% em %}になったら変更回数を答える．

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
	int n;
	while(cin >> n && n) {
		vector<string> v(n), v2(n);
		rep(i, n) cin >> v[i] >> v2[i];

		string s, t;
		cin >> s >> t;

		queue<pair<string, int> > que;
		que.push(mp(s, 0));
		int ans = -1;

		while(que.size()) {
			pair<string, int> p = que.front();
			que.pop();

			if(p.first == t) {
				ans = p.second;
				break;
			}

			rep(i, n) {
				bool flag = false;
				string s2 = p.first;
				string::size_type id = s2.find(v[i]);
				while(id != string::npos) {
					flag = true;
					s2.replace(id, v[i].size(), v2[i]);
					id = s2.find(v[i], id + v2[i].size());
				}

				if(flag && s2.size() <= t.size()) {
					que.push(mp(s2, p.second + 1));
				}
			}
		}

		cout << ans << endl;
	}

	return 0;
}
```

