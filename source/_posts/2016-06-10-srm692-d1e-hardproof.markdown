---
layout: post
title: "SRM692 D1E HardProof"
date: 2016-06-10 22:53:39 +0900
comments: true
categories: [topcoder, srm, グラフ, 強連結成分分解, しゃくとり法]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-branding="0" data-card-type="article-full"><h4><a href="https://community.topcoder.com/stat?c=problem_statement&pm=14334&rd=16747">TopCoder Statistics - Problem Statement</a></h4><p>You are preparing for an exam in Calculus 4. In the chapter "Abstract Fourier Series", you encountered an excercise in which you are given a collection of N statements and your task is to prove that all N statements are equivalent. For the purpose of this problem, we will number the statements 0 through N-1.</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

$0 => 1$を頂点$0$ $\to$ 頂点$1$の有向グラフと考えると，$N$個の頂点を含む閉路の中でその辺の最大コスト$-$最小コストを答えれば良いとなる．  
  
コストが近いものを同士を使った方が良いので，使うコストの端を持ってしゃくとり法をした．（考え方は[ここ]({% post_url 2016-05-21-aoj1280-slim-span %})に似てると思った）  
$N$個の頂点を含む閉路の検出は強連結成分分解した後の連結成分数が$1$であるかどうかで判定．  
全て辺を張ってで行けるか行けないを持ったらTLE．辺を追加したり削除したりした場合でもTLE．毎回毎回グラフを作り直したらACした．(最初の$2$つは$N ^2$よりもかかっている(?)よく分かっていない)

# Code

```cpp
struct edge {
	int from,to;
	int cost;

	edge(int t,int c) : to(t),cost(c) {}
	edge(int f,int t,int c) : from(f),to(t),cost(c) {}

	bool operator<(const edge &e) const {
		return cost < e.cost;
	}
};

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

	int build() {
		memset(used, 0, sizeof(used));
		rep(i, n) {
			if (!used[i]) dfs(i);
		}

		memset(used, 0, sizeof(used));
		int k = 0;
		for (int i = vs.size()-1; i >= 0; i--) {
			if (!used[vs[i]]) rdfs(vs[i], k++);
		}

		// ng_make(k);
		return k;
	}
};

class HardProof {
	public:
	int minimumCost(vector<int> D) {
		int n = sqrt(D.size());

		int k = -1;
		map<int, vector<P> > m;

		rep(i, n) {
			rep(j, n) {
				k++;
				if(i == j) continue;
				m[D[k]].push_back(mp(i, j));
			}
		}

		int ans = INF;
		map<int, vector<P> >::iterator l = m.begin(), r = m.begin(), ite;

		for(r = m.begin(); r != m.end(); r++) {
			k = 1;
			while(k == 1) {
				SCC scc(n);
				ite = l;
				while(ite != m.end() && ite->first <= r->first) {
					vector<P> v = ite->second;
					rep(i, v.size()) {
						int a = v[i].first, b = v[i].second;
						scc.add(a, b);
					}

					if(ite == r) break;
					ite++;
				}

				k = scc.build();
				if(k == 1) {
					ans = min(ans, r->first - l->first);
					l++;
					continue;
				}
				break;
			}
		}

		if(ans == INF) return 0;
		return ans;
	}
};
```

