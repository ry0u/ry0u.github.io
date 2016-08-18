---
layout: post
title: "AOJ1251 Pathological Paths"
date: 2016-03-26 22:12:52 +0900
comments: true
categories: [AOJ-ICPC, 300, 木]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-branding="0" data-card-type="article"><h4><a href="http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=1251">Pathological Paths</a></h4><p>Professor Pathfinder is a distinguished authority on the structure of hyperlinks in the World Wide Web. For establishing his hypotheses, he has been developing software agents, which automatically traverse hyperlinks and analyze the structure of the Web. Today, he has gotten an intriguing idea to improve his software agents.</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

木構造でディレクトリ構造を表す．探すパスが両方葉の場合，ノードの番号が一致するかを見る．葉ではなくディレクトリの場合は，自分の子に文字列が"index.html"であり葉であるノードがあるかを見る．ない場合は"not found"で，ある場合はそのノード番号を比較する．

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

int cnt = 0;

struct Tree {
	string s;
	int id;
	Tree* par;
	vector<Tree*> v;

	Tree(Tree* par, string s, int id) : par(par), s(s), id(id) {}

	void add(int i, vector<string> res) {
		if(i == res.size()) return;

		bool flag = true;
		rep(j, v.size()) {
			if(v[j]->s == res[i]) flag = false;
		}

		if(flag) {
			cnt++;
			Tree *node = new Tree(this, res[i], cnt);
			v.push_back(node);
		}

		rep(j, v.size()) {
			if(v[j]->s == res[i]) {
				v[j]->add(i+1, res);
			}
		}
	}

	Tree* find(int i, vector<string> res) {
		// cout << " -- in find :" << s << endl;
		if(i == res.size()) {
			return this;
		}

		if(res[i] == ".") {
			if(v.size() == 0) return NULL;
			return find(i+1, res);
		} else if(res[i] == "..") {
			if(v.size() == 0) return NULL;
			if(par == NULL) return NULL;
			return par->find(i+1, res);
		} else if(res[i] == "") {
			if(v.size() == 0) return NULL;
			return find(i+1, res);
		} else {
			rep(j, v.size()) {
				if(v[j]->s == res[i]) {
					return v[j]->find(i+1, res);
				}
			}
			return NULL;
		}
	}

};

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
	int n, m;
	while(cin >> n >> m) {
		if(n == 0 && m == 0) break;

		cnt = 0;
		Tree *root = new Tree(NULL, "root", cnt);

		rep(i, n) {
			string s;
			cin >> s;

			vector<string> ret = split(s, '/');

			root->add(0, vector<string>(ret.begin()+1, ret.end()));
		}

		rep(q, m) {
			string s, t;
			cin >> s >> t;

			vector<string> r = split(s, '/');
			vector<string> r2 = split(t, '/');

			Tree *node = root->find(0, r);
			Tree *node2 = root->find(0, r2);

			// if(node == NULL) cout << " node 1 : null" << endl;
			// if(node2 == NULL) cout << " node 2 : null" << endl;
		
			if(node == NULL || node2 == NULL) {
				cout << "not found" << endl;
			} else {

				Tree *res = NULL;
				if(node->v.size() == 0) {
					res = node;
				} else {
					rep(i, node->v.size()) {
						if(node->v[i]->v.size() == 0 && node->v[i]->s == "index.html") {
							res = node->v[i];
						}
					}
				}

				Tree *res2 = NULL;
				if(node2->v.size() == 0) {
					res2 = node2;
				} else {
					rep(i, node2->v.size()) {
						if(node2->v[i]->v.size() == 0 && node2->v[i]->s == "index.html") {
							res2 = node2->v[i];
						}
					}
				}

				// if(res == NULL) cout << "NULL";
				// else cout << res->s << ", " << res->id;
				//
				// cout << " ";
				//
				// if(res2 == NULL) cout << "NULL";
				// else cout << res2->s << ", " << res2->id;
				//
				// cout << endl;

				if(res == NULL || res2 == NULL) {
					cout << "not found" << endl;
				} else if(res->id == res2->id && res->s == res2->s) {
					cout << "yes" << endl;
				} else {
					cout << "no" << endl;
				}
			}
		}
	}

	return 0;
}
```

