---
layout: post
title: "Manthan, Codefest 16D Fibonacci-ish"
date: 2016-02-29 15:59:40 +0900
comments: true
categories: [Codeforces, シュミレーション]
---
<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-chrome="0" data-card-controls="0"><h4><a href="http://codeforces.com/contest/633/problem/D">Problem - D - Codeforces</a></h4><p>Yash has recently learnt about the Fibonacci sequence and is very excited about it. He calls a sequence Fibonacci-ish if You are given some sequence of integers . Your task is rearrange elements of this sequence in such a way that its longest possible prefix is Fibonacci-ish sequence.</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

やることは， {% m %} f\_0とf\_1 {% em %}選び，シュミレーションする．本番中はpretest 3でTLEを出して，これじゃあ間に合わないと考えていたが，単純に{% m %} 0 {% em %}を考慮していないためだった．  
最初に {% m %} 0 {% em %}をはじいて，全て{% m %} 0 {% em %}のみのパターンか最初に{% m %} 0 {% em %}をつけるパターンのみでいいと思っていたが，途中で{% m %} 0 {% em %}を経由するパターンも普通にあってそこに気づかなかった．  
またsetに突っ込んでその数があるかを確認していたけど，同じ数が出てくるパターンがあるので最初にどの数が何個あるかをmapで持たなければいけなかった．  
stackに積んでおけば後に続く項の個数が分かるので
{% math %}
	memo[mp(a, b)] := (a, b)の時の後に続く項の個数
{% endmath %}
とかやるのかな思ったけど，(a, b)にいたるまでに使ってきた数が違えば後に続く項も違いダメだった．  

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

int main() {
	int n;
	cin >> n;

	vector<ll> v(n);
	map<ll, int> m;
	set<int> st;

	rep(i, n) {
		cin >> v[i];
		m[v[i]]++;
		st.insert(v[i]);
	}

	int ans = m[0];
	rep(i, n) {
		rep(j, n) {
			ll a = v[i], b = v[j], c;

			if(i == j) continue;
			if(a == 0 && b == 0) continue;

			m[a]--;
			m[b]--;

			stack<ll> S;
			S.push(a);
			S.push(b);

			while(st.find(a + b) != st.end() && m[a + b] > 0) {
					c = a + b;
					a = b;
					b = c;

					m[c]--;
					S.push(c);
			}

			ans = max(ans, (int)S.size());

			while(S.size()) {
				m[S.top()]++;
				S.pop();
			}
		}
	}

	cout << ans << endl;

	return 0;
}
```
