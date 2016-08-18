---
layout: post
title: "AOJ1305 Membership Management"
date: 2016-03-23 20:07:05 +0900
comments: true
categories: [AOJ-ICPC, 300]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-type="article" data-card-branding="0"><h4><a href="http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=1305">Membership Management</a></h4><p>Peter is a senior manager of Agile Change Management (ACM) Inc., where each employee is a member of one or more task groups. Since ACM is agile, task groups are often reorganized and their members frequently change, so membership management is his constant headache.</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

group1のmenberを順番にstackに入れていく．menberがgroupの時は，そのgroupを全てstackに入れる．重複がないように確定したメンバーはsetに突っ込む．

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

int main() {
	int n;
	while(cin >> n && n) {
		vector<string> s(n);
		rep(i, n) cin >> s[i];

		map<string, set<string> > m;
		map<string, int> id;
		vector<string> res[n];
		rep(i, n) {
			vector<string> ret = split(s[i], ':');
			id[ret[0]] = i;
			string t = "";
			rep(j, ret[1].size()) {
				if(ret[1][j] == ',' || ret[1][j] == '.') {
					m[ret[0]].insert(t);
					res[i].push_back(t);
					t = "";
				} else {
					t += ret[1][j];
				}
			}
		}

		set<string> S;
		stack<string> st;
		bool used[105];
		memset(used, 0, sizeof(used));
		used[0] = true;

		rep(i, res[0].size()) {
			st.push(res[0][i]);

			while(st.size()) {
				string tp = st.top();
				st.pop();

				if(m.count(tp) == 0) {
					S.insert(tp);
				} else if(!used[id[tp]]){
					used[ id[tp] ] = true;

					set<string>::iterator ite;
					for(ite = m[tp].begin(); ite != m[tp].end(); ite++) {
						st.push(*ite);
					}
				}
			}
		}

		cout << S.size() << endl;
	}

	return 0;
}
```

