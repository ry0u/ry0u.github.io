---
layout: post
title: "Codeforces353-div2D Tree Construction"
date: 2016-05-17 18:36:48 +0900
comments: true
categories: [Codeforces, 木]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-branding="0" data-card-type="article"><h4><a href="http://codeforces.com/contest/675/problem/D">Problem - D - Codeforces</a></h4><p>During the programming classes Vasya was assigned a difficult problem. However, he doesn't know how to code and was unable to find the solution in the Internet, so he asks you to help. You are given a sequence a, consisting of n distinct integers, that is used to construct the binary search tree.</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

　{% m %} 2 {% em %}分木の挿入する順番が与えられる．挿入した時の親の番号を答える．  
愚直に木を構成すると線のような木の場合に {% m %} O(n ^2) {% em %}となるので間に合わない．区間をsetで管理すると次に挿入する場所が {% m %} logn {% em %}で持ってこれるので，全体で {% m %} O(n logn) {% em %}となる．map({% m %}l, r {% em %})に区間 {% m %} [l, r] {% em %}の親を持っておくようにした．  

sample2でどのように動くかをメモしておく．(書いておかないと絶対忘れる．．．)

# Sample 2
> {% m %} 5 {% em %}  
> {% m %} 4\ 2\ 3\ 1\ 6\ {% em %}

手順，挿入する数，挿入する区間 {% m %} \to {% em %} 挿入した結果と書いてみた．

{% math %}
\begin{eqnarray}
insert \ \ (4) \ \ [-\infty, \infty] \ \ &\to& \ \ [-\infty, 4], [4, \infty] \\
insert \ \ (2) \ \ [-\infty,\ \ 4] \ \ &\to& \ \ [-\infty, 2], [2, 4], [4, \infty]  \\
insert \ \ (3) \ \ [\,\ \ \ \ 2,\ \ 4] \ \ &\to& \ \ [-\infty, 2], [2, 3], [3, 4], [4, \infty] \\
insert \ \ (1) \ \ [-\infty,\ \ 2] \ \ &\to& \ \ [-\infty, 1], [1, 2], [2, 3], [3, 4], [4, \infty] \\
insert \ \ (6) \ \ [\,\ \ \ \ 4,\infty] \ \ &\to& \ \ [-\infty, 1], [1, 2], [2, 3], [3, 4], [4, 6], [6, \infty] \\
\end{eqnarray}
{% endmath %}

後は区間{% m %} [l, r] {% em %}のmapに入っている値を答えて， {% m %} [l, v[i]], [v[i], r] {% em %}に {% m %} v[i] {% em %}を入れておく．

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

map<P, int> m;

int main() {
	int n;
	cin >> n;

	vector<int> v(n);
	rep(i, n) cin >> v[i];

	vector<int> ans;

	set<int> st;
	st.insert(INF);
	st.insert(-INF);
	st.insert(v[0]);

	m[mp(-INF, v[0])] = v[0];
	m[mp(v[0], INF)] = v[0];

	REP(i, 1, n) {
		set<int>::iterator ite = st.upper_bound(v[i]);
		int r = *ite;
		ite--;
		int l = *ite;

		ans.push_back(m[mp(l, r)]);

		m[mp(l, v[i])] = v[i];
		m[mp(v[i], r)] = v[i];
		st.insert(v[i]);
	}

	rep(i, ans.size()) {
		cout << ans[i];

		if(i == ans.size()-1) cout << endl;
		else cout << " ";
	}


	return 0;
}
```

