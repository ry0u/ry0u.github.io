---
layout: post
title: "SRM691 D2E PlusoneGame"
date: 2016-05-31 13:50:42 +0900
comments: true
categories: [topcoder, srm]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-branding="0" data-card-type="article-full"><h4><a href="https://community.topcoder.com/stat?c=problem_statement&pm=14303&rd=16730">TopCoder Statistics - Problem Statement</a></h4><p>Hero plays a game with a deck of cards and a counter. Initially, the counter is set to zero. During the game Hero must play each card in the deck exactly once. He gets to choose the order in which he plays the cards. You are given the description of the deck in the String s.</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

文字列 {% m %} s {% em %}が与えられる． {% m %} + {% em %}の場合はカウンターをインクリメント， {% m %} 0 \sim 9 {% em %}の場合は現在のカウンターとその桁の数の差の絶対値分，ペナルティがたまる．ペナルティが最小にように文字列を並び替えたい．  
  
各桁を引く前にカウンターがその数と同じだけたまっている状態になっていれば，ペナルティは {% m %} 0 {% em %}となる． なので，{% m %} + {% em %}があるだけ{% m %} 0+11111+2222222+333333+44444\dots {% em %}のように交互に並べ，足りない場合は {% m %} + {% em %}無しで，余った場合は最後につけた．

# Code

```cpp
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>
#include <sstream>
#include <cstring>
#include <queue>
#include <set>
#include <map>

#define REP(i,k,n) for(int i=k;i<n;i++)
#define rep(i,n) for(int i=0;i<n;i++)

using namespace std;
typedef long long ll;

class Plusonegame {
	public:
	string getorder(string s) {
		int n = s.size();
		map<char, int> m;
		int cnt = 0;

		rep(i, n) {
			if(s[i] == '+') cnt++;
			else {
				m[s[i]]++;
			}
		}

		string ans = "";
		rep(i, 10) {
			char c = '0' + i;

			rep(j, m[c]) {
				ans += c;
			}

			if(cnt > 0) {
				ans += '+';
				cnt--;
			}
		}

		while(cnt) {
			ans += '+';
			cnt--;
		}

		return ans;
	}
};
```

