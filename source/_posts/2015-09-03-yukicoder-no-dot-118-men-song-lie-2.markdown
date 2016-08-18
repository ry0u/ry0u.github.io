---
layout: post
title: "yukicoder No.118 門松列(2)"
date: 2015-09-03 22:00:55 +0900
comments: true
categories: [yukicoder,星2]
---

http://yukicoder.me/problems/221  

# 考察
今回は与えられるA_{i}が100と小さいので，門松の要素の列挙が出来る．何組の選び方があるあを求めたいので，それぞれの長さが何本あるかを持っておき，一番小さいもの，真ん中，一番大きいものの個数をかける．探す範囲はN本与えられた長さの最小値から最大値までで行った．mod 10^9 + 7を忘れない．

# Code

```cpp
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>
#include <cstring>
#include <map>

#define REP(i,k,n) for(int i=k;i<n;i++)
#define rep(i,n) for(int i=0;i<n;i++)
#define INF 1<<30
#define MOD 1000000007

using namespace std;
typedef long long ll;

int main()
{
	int n;
	cin >> n;
	
	vector<int> v(n);
    map<int,ll> m;
    int vmin = INF, vmax = 0;
    rep(i,n) {
        cin >> v[i];
        m[v[i]]++;

        vmin = min(vmin,v[i]);
        vmax = max(vmax,v[i]);
    }

    ll ans = 0;
    REP(i,vmin,vmax+1) {
        REP(j,i+1,vmax+1) {
            REP(k,j+1,vmax+1) {
                ans += m[i] * m[j] * m[k];
                ans %= MOD;
            }
        }
    }

    cout << ans << endl;


	return 0;
}
```

