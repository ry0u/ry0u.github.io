---
layout: post
title: "Codeforces314-div2c Geometric Progression"
date: 2015-08-06 09:34:01 +0900
comments: true
categories: [codeforces]
---

http://codeforces.com/contest/567/problem/C  
N個の数字と比kが与えられる．比がkである長さ3の部分数列の数を求めたい．

# 考察
3つの等比数列の真ん中の値に注目する．注目している数をxと置くと，d = x/k，x，u = x\*kの数列の数を知ることが出来れば良い．自分の前にあるdの要素数，自分の後ろにあるuの要素数の乗算で，今見ているxを含む部分数列の数が分かる．これをN個に対して全てやった．Nは最大2\*10^5のため，オーバーフローに注意する．

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

    ll n,k;
    cin >> n >> k;

    vector<ll> v(n);
    rep(i,n) {
        cin >> v[i];
    }

    map<ll,ll> mup;
    rep(i,n) {
        mup[v[i]]++;
    }

    map<ll,ll> mdown;

    ll ans = 0;

    rep(i,n) {
        mup[v[i]]--;
        if(v[i] % k == 0) {
            ll d = v[i]/k;
            ll u = v[i]*k;

            ans += mdown[d] * mup[u];
        }
        mdown[v[i]]++;
    }

    cout << ans << endl;

    return 0;
}
```
