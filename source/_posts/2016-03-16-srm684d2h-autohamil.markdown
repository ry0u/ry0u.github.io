---
layout: post
title: "SRM684D2H Autohamil"
date: 2016-03-16 00:01:41 +0900
comments: true
categories: [srm, topcoder, 強連結成分分解, グラフ]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-type="article" data-card-branding="0"><h4><a href="https://community.topcoder.com/stat?c=problem_statement&pm=14183&rd=16688">TopCoder Statistics - Problem Statement</a></h4><p>In this problem, all strings are binary strings. That is, each character of a string is either '0' or '1'. A deterministic finite automaton is a machine that processes strings. The automaton has a finite set of possible states. The states are numbered 0 through n-1, where n is the number of states.</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

有向グラフが与えられるので，ハミルトン路があるかどうかを判定せよ．

---

閉路があるので，強連結成分分解する．分解後のグラフで{% m %} 0 {% em %}を始点に探索を始め，分かれ道があれば出来ないと思ったが，全然違うしサンプルも合わない．こういう場合は出来る．  

{% img /images/SRM/684d2h.png %}
  

強連結成分分解をしDAGになったので，トポロジカルソートしてトポロジカル順序で{% m %} i \to i+1 {% em %}の辺があるかを調べる．また，トポロジカル順序の開始が{% m %} 0 {% em %}で無い場合も出来ないことを忘れない(分からなかった)．


# Code
```cpp
struct SCC {
	int n;
	vector<vector<int> > g, rg, ng, scc; // rg: 逆グラフ, ng: 分解後のグラフ
	vector<int> res; // scc: 強連結成分に属する頂点, res:強連結成分の番号
	bool used[100005];

	SCC(int _n) {
		n = _n;
		g.resize(n); rg.resize(n) ; scc.resize(n); res.resize(n);
	}

	SCC(const vector<vector<int> > &g) : n(g.size()), g(g), rg(n), scc(n), res(n) {
		rep(i, n) {
			rep(j, g[i].size()) rg[g[i][j]].push_back(i);
		}
	}

	// i-jに辺を追加する
	void add(int i, int j) {
		g[i].push_back(j);
		rg[j].push_back(i);
	}

	vector<int> vs;
	void dfs(int v) {
		used[v] = true;
		rep(i, g[v].size()) {
			if(!used[ g[v][i] ]) dfs(g[v][i]);
		}
		vs.push_back(v);
	}

	void rdfs(int v, int k) {
		used[v] = true;
		res[v] = k; 
		scc[k].push_back(v);
		rep(i, rg[v].size()) {
			if(!used[ rg[v][i] ]) rdfs(rg[v][i], k);
		}
	}

	void ng_make(int k) {
		ng.resize(k);
		rep(i, n) {
			set<int> S;
			rep(j, g[i].size()) {
				int to = g[i][j];
				if(res[i] == res[to]) continue;
				if(S.find(res[to]) != S.end()) continue;
				ng[res[i]].push_back(res[to]);
				S.insert(res[to]);
			}
		}
	}

	int run() {
		memset(used, 0, sizeof(used));
		rep(i, n) {
			if (!used[i]) dfs(i);
		}

		memset(used, 0, sizeof(used));
		int k = 0;
		for (int i = vs.size()-1; i >= 0; i--) {
			if (!used[vs[i]]) rdfs(vs[i], k++);
		}

		ng_make(k);
		return k;
	}
};

bool used[55];
vector< vector<int> > ng;
vector<int> out;

void dfs(int cur) {
	used[cur] = true;
	rep(i, ng[cur].size()) {
		int v = ng[cur][i];
		if(!used[v]) dfs(v);
	}
	out.push_back(cur);
}

class Autohamil {
	public:
	string check(vector <int> z0, vector <int> z1) {
		int n = z0.size();
		SCC scc(n);

		rep(i, n) {
			if(i != z0[i]) scc.add(i, z0[i]);
			if(i != z1[i]) scc.add(i, z1[i]);
		}

		int m = scc.run();
		ng.resize(m);
		rep(i, m) {
			ng[i].resize(scc.ng[i].size());
			rep(j, scc.ng[i].size()) {
				ng[i][j] = scc.ng[i][j];
			}
		}

		int cnt[55];
		memset(cnt, 0, sizeof(cnt));
		rep(i, m) {
			rep(j, ng[i].size()) {
				cnt[ng[i][j]]++;
			}
		}

		REP(i, 1, m) {
			if(cnt[i] == 0) return "Does not exist";
		}

		int s = scc.res[0];
		memset(used, 0, sizeof(used));
		out.clear();
		dfs(s);
		reverse(out.begin(), out.end());

		if(out[0] != 0) return "Does not exist";

		rep(i, out.size()-1) {
			bool ch = false;
			rep(j, ng[out[i]].size()) {
				if(ng[out[i]][j] == out[i+1]) {
					ch = true;
				}
			}

			if(ch) continue;
			return "Does not exist";
		}
		return "Exists";
	}
};
```

