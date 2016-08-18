---
layout: post
title: "Manthan, Codefest 16C Spy Syndrome 2"
date: 2016-02-29 15:32:14 +0900
comments: true
categories: [Codeforces, 動的計画法, Trie]
---
<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-chrome="0" data-card-controls="0"><h4><a href="http://codeforces.com/contest/633/problem/C">Problem - C - Codeforces</a></h4><p>After observing the results of Spy Syndrome, Yash realised the errors of his ways. He now believes that a super spy such as Siddhant can't use a cipher as basic and ancient as Caesar cipher. After many weeks of observation of Siddhant's sentences, Yash determined a new cipher technique.</p></blockquote>

<!-- more -->

原文を全て小文字にしてreverseして，接合した文を復元する．  
まずTrie木を作って文字列が単語リストにあるかを判定出来るようにする．  
次に，
{% math %}
	dp[i] := i番目の文字まで復元可能
{% endmath %}
とする．Trieのfind関数にその単語そのものがあればTrue, また現在のノードがendであり，その場所の{% m %} dp[i] {% em %}をみて復元可能な時にTrueを返す．その文字列を{% m %} S[i] {% em %}にとっておく．  
後は末尾から帰ってくれば原文の単語の逆順がわかるのでreverseする．

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

string S[10005], ret = "", res = "";
bool dp[10005];

struct Trie {
	Trie *next[26];
	bool end;

	Trie() {
		fill(next,next+26,(Trie *)0);
		end = false;
	}

	void insert(string &s,int i) {
		if(i == s.size()) {
			this->end = true;
			return;
		}

		if(this->next[s[i]-'a'] == NULL) {
			this->next[s[i]-'a'] = new Trie();
		}

		this->next[s[i]-'a']->insert(s,i+1);
	}

	bool find(int s, int i) {
		if(s - i >= 0 && dp[s - i] && this->end) {
			string t = "";
			rep(j, i) {
				t += ret[s-j];
			}
			res = t;
			return true;
		}

		if((s + 1) - i == 0) {
			if(this->end) {
				string t = "";
				rep(j, i) {
					t += ret[s-j];
				}
				res = t;
				return true;
			}
			else return false;
		}

		if(this->next[ret[s-i]-'a'] != NULL) {
			if(this->next[ret[s-i]-'a']->find(s, i+1)) return true;
		}

		return false;
	}
};


int main() {
	int n;
	cin >> n;

	string s;
	cin >> s;

	int m;
	cin >> m;

	vector<string> v(m);
	map<string, string> f;
	Trie *trie = new Trie();

	rep(i, m) {
		cin >> v[i];

		string t = v[i];
		rep(j, t.size()) {
			t[j] = tolower(t[j]);
		}

		f[t] = v[i];
		trie->insert(t, 0);
	}

	memset(dp, 0, sizeof(dp));

	rep(i, n) {
		ret = ret + s[i];

		if(trie->find(i, 0)) {
			S[i] = f[res];
			dp[i] = true;
		}
	}

	vector<string> ans;
	int cur = n-1;
	while(dp[cur]) {
		string t = S[cur];
		ans.push_back(t);
		cur -= t.size();
	}

	reverse(ans.begin(), ans.end());

	rep(i, ans.size()) {
		cout << ans[i];
		if(i == ans.size()-1) cout << endl;
		else cout << " ";
	}

	return 0;
}
```
