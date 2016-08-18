---
layout: post
title: "AOJ1250 Leaky Cryptography"
date: 2016-03-22 21:00:41 +0900
comments: true
categories: [AOJ-ICPC, 250]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-type="article" data-card-branding="0"><h4><a href="http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=1250">Leaky Cryptography</a></h4><p>The ACM ICPC judges are very careful about not leaking their problems, and all communications are encrypted. However, one does sometimes make mistakes, like using too weak an encryption scheme. Here is an example of that. The encryption chosen was very simple: encrypt each chunk of the input by flipping some bits according to a shared key.</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

一番下の桁から順番に決めていく．今までの {% m %} \rm{sum} {% em %}の {% m %} i {% em %}桁目と {% m %} N_1 〜 N_8{% em %}の{% m %} i {% em %}桁の和が {% m %} N_9 {% em %}の {% m %} i {% em %}桁と一致しているかを見る．一致していない場合に， {% m %} 8 {% em %}つの{% m %} i {% em %}桁目を反転して，答えの {% m %} i {% em %}桁目のビットを立てる．

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
#include <bitset>

#define REP(i,k,n) for(int i=k;i<n;i++)
#define rep(i,n) for(int i=0;i<n;i++)
#define INF 1<<30
#define pb push_back
#define mp make_pair

using namespace std;
typedef long long ll;
typedef pair<int,int> P;

ll f(string s) {
	ll res = 0, t = 1;

	for(int i = s.size()-1; i >= 0; i--) {
		if('0' <= s[i] && s[i] <= '9') {
			res += (s[i] - '0') * t;
		} else {
			res += (10 + (s[i] - 'a')) * t;
		}
		t *= 16;
	}
	return res;
}

int main() {
	int n;
	cin >> n;

	rep(q, n) {
		vector<string> v(9);
		vector<ll> t(9);
		rep(i, 9) {
			cin >> v[i];
			t[i] = f(v[i]);
		}

		ll ans = 0;

		vector< vector<int> > bit(9, vector<int>(32));
		rep(i, 9) {
			rep(j, 32) {
				if(t[i] & (1LL << j)) {
					bit[i][j] = 1;
				}
			}
		}

		ans = 0;
		ll sum = 0;
		rep(i, 32) {
			ll d = 0;
			rep(j, 8) {
				d += bit[j][i];
			}
			bitset<32> ss(sum);

			if( ((((sum >> i) & 1) + d) & 1) == bit[8][i]) {
				sum += (d << i);
			} else {
				d = 0;
				rep(j, 8) {
					d += bit[j][i] ^ 1;
				}
				sum += (d << i);
				ans += (1LL << i);
			}
		}

		cout << hex << ans << endl;
	}

	return 0;
}
```

