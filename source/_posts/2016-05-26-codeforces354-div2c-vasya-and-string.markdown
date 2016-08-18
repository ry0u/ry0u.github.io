---
layout: post
title: "Codeforces354-div2C Vasya and String"
date: 2016-05-26 09:38:50 +0900
comments: true
categories: [Codeforces, しゃくとり法]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-branding="0" data-card-type="article"><h4><a href="http://codeforces.com/contest/676/problem/C">Problem - C - Codeforces</a></h4><p>High school student Vasya got a string of length n as a birthday present. This string consists of letters 'a' and '' only. Vasya denotes beauty of the string as the maximum length of a substring (consecutive subsequence) consisting of equal letters. Vasya can change no more than k characters of the original string.</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

長さ{% m %} n {% em %}の文字{% m %} a, b {% em %}で構成された文字列が与えられる．{% m %} k {% em %}回変更可能な時に，同じ文字で構成される部分文字列の最大長を答える．  
  
文字 {% m %} a {% em %}だけで構成される部分文字列を考える．区間{% m %} [l, r]{% em %}の中に文字{% m %} b {% em %}が {% m %} k {% em %}個以下ならば，その区間では全て {% m %} a {% em %}にすることができる．これをずらしながらやっていく．文字{% m %} b {% em %}で構成される部分文字列も同様にやり，最大値を取った． {% m %} O(n) {% em %}．


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

int main() {
	int n, k;
	cin >> n >> k;

	string s;
	cin >> s;

	int ans = 0;

	int l = 0, r = 0, cnt = 0;
	rep(i, n) {
		r = i;
		if(s[i] == 'b') cnt++;

		if(cnt <= k) {
			ans = max(ans, r - l + 1);
		}

		while(cnt > k) {
			if(s[l] == 'b') cnt--;
			l++;
		}
	}

	l = 0, r = 0, cnt = 0;
	rep(i, n) {
		r = i;
		if(s[i] == 'a') cnt++;

		if(cnt <= k) {
			ans = max(ans, r - l + 1);
		}

		while(cnt > k) {
			if(s[l] == 'a') cnt--;
			l++;
		}
	}

	cout << ans << endl;


	return 0;
}
```

本番では落ちた．区間 {% m %} [l, r] {% em %}の中の{% m %} b {% em %}が{% m %} k {% em %}を超えた時，というのは今見ている文字が{% m %} b {% em %}でその文字を {% m %} a {% em %}に変えて， {% m %} l {% em %}をずらす，ということなので {% m %} r {% em %}は必ず今見ている場所 {% m %} i {% em %}となる．ということがよく整理できていなかった．反省．
