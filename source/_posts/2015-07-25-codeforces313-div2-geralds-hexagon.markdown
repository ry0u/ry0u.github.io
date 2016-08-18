---
layout: post
title: "Codeforces313-div2 Gerald's Hexagon"
date: 2015-07-25 17:40:19 +0900
comments: true
categories: [codeforces,幾何]
---

六角形の辺の長さが時計回りの順番で与えられる．長さ1の正三角形がいくつあるかを求めよ．

# 考察
まずSample1を見る  
{% img /images/Codeforces313-div2/image1.png %}  
これは正六角形であるため，この中に正三角形は6個．これのまず大きな三角形としてみる．追加した三角形を青で表すと  
{% img /images/Codeforces313-div2/image2.png %}  

大きな三角形は高さをnとすると，n段目の高さは1 + (n-1)*2で計算できる．そして同じ計算方法で追加した3つの三角形を引く．これで計算できる．

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
    int a,b,c,d,e,f;
    cin >> a >> b >> c >> d >> e >> f;

    int len = max(a+b+c,a+e+f);
    int ans = 0;
    int t = 1;

    rep(i,len) {
        ans += t;
        t += 2;
    }

    t = 1;
    rep(i,a) {
        ans -= t;
        t += 2;
    }

    t = 1;
    rep(i,c) {
        ans -= t;
        t += 2;
    }

    t = 1;
    rep(i,e) {
        ans -= t;
        t += 2;
    }

    cout << ans << endl;


    return 0;
}
```

