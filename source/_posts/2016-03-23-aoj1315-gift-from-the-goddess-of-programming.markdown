---
layout: post
title: "AOJ1315 Gift from the Goddess of Programming"
date: 2016-03-23 21:21:54 +0900
comments: true
categories: [AOJ-ICPC, 300]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-type="article" data-card-branding="0"><h4><a href="http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=1315">Gift from the Goddess of Programming</a></h4><p>The goddess of programming is reviewing a thick logbook, which is a yearly record of visitors to her holy altar of programming. The logbook also records her visits at the altar. The altar attracts programmers from all over the world because one visitor is chosen every year and endowed with a gift of miracle programming power by the goddess.</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>


<!-- more -->

女神と被っている時間をそれぞれ出して，最大値を出力する．同じ日にちかどうか，時間がどう被っているかを注意しながらやったがバグを何個もしまった．

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

vector<string> split(const string &str, char delim) {
	vector<string> res;
	size_t current = 0, found;
	while((found = str.find_first_of(delim, current)) != string::npos) {
		res.push_back(string(str, current, found - current));
		current = found + 1;
	}
	res.push_back(string(str, current, str.size() - current));
	return res;
}


// s1 - s2
int f(string s1, string s2) {
	int a = (s1[0] - '0') * 10 + (s1[1] - '0');
	int b = (s1[3] - '0') * 10 + (s1[4] - '0');
	int c = (s2[0] - '0') * 10 + (s2[1] - '0');
	int d = (s2[3] - '0') * 10 + (s2[4] - '0');

	if(a == c) {
		return b - d;
	} else {
		return (a - (c + 1)) * 60 + (60 - d) + b;
	}
}

int main() {
	int n;
	while(cin >> n && n) {

		map<string, vector<pair<string, string> > > IN, OUT;

		rep(i, n) {
			string a, b, c, d;
			cin >> a >> b >> c >> d;

			if(c == "I") {
				IN[d].push_back(mp(a, b));
			} else {
				OUT[d].push_back(mp(a, b));
			}
		}

		int ans = 0;
		vector<pair<string, string> > gin = IN["000"];
		vector<pair<string, string> > gout = OUT["000"];


		map<string, vector<pair<string, string> > >::iterator in, out;
		for(in = IN.begin(), out = OUT.begin(); in != IN.end() && out != OUT.end(); in++, out++) {
			if(in->first == "000") continue;

			vector<pair<string, string > > v = in->second;
			vector<pair<string, string > > v2 = out->second;

			int res = 0;
			rep(i, v.size()) {
				rep(j, gin.size()) {
					if(v[i].first != gin[j].first) continue;
			
					string a = v[i].second, b = v2[i].second, c = gin[j].second, d = gout[j].second;
					if(c <= a && b <= d) {
						res += f(b, a);
					} else if(a <= c && d <= b) {
						res += f(d, c);
					} else if(c <= a && a <= d && d <= b) {
						res += f(d, a);
					} else if(a <= c && c <= b && b <= d) {
						res += f(b, c);
					}
				}
			}

			ans = max(ans, res);
		}

		cout << ans << endl;
	}

	return 0;
}
```

