---
layout: post
title: "Codeforces314-div2D One-Dimensional Battle Ships"
date: 2015-08-10 11:21:11 +0900
comments: true
categories: [codeforces, 2分探索]
---

http://codeforces.com/contest/567/problem/D  
1次元のマス目がある．ここに長さaの船をk個置く．Bobの質問に対して，Aliceが嘘を言った時の番号を答える．

# 考察
どの質問までかを2分探索する．この探索に対してのvectorを作り，それぞれの区間に船が何個入るかを出す．区間を見たいので，端の0のn+1を追加する．この個数がkをこえるかこえないかで判断する．計算量は，探索でlog(n)，vectorのsortにn\*log(n)かかるので，全体でO(n\*log(n)^2)．

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
    int n,k,a;
    cin >> n >> k >> a;

    int m;
    cin >> m;

    vector<int> v(m);
    rep(i,m) {
        cin >> v[i];
    }

    int l = 0,r = m+1;
    while(r - l > 1) {
        int mid = (l+r)/2;
        vector<int> t(v.begin(),v.begin()+mid);

        t.push_back(0);
        t.push_back(n+1);
        sort(t.begin(),t.end());

        int cnt = 0;
        rep(j,t.size()-1) {
            int d = t[j+1] - t[j];
            cnt += d/(a+1);
        }

        if(cnt >= k) {
            l = mid;
        }
        else {
            r = mid;
        }
    }

    if(l == m) cout << -1 << endl;
    else cout << l+1 << endl;

    return 0;
}
```

