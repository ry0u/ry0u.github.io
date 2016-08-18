---
layout: post
title: "Codeforces312-div2C Amr and Chemistry"
date: 2015-07-15 03:38:42 +0900
comments: true
categories: [codeforces]
---

http://codeforces.com/contest/558/problem/C  
N個のデータが与えられる．それぞれの値には*2，/2することが出来る．全ての要素を揃えるための最小手数を求めよ

# 考察
この操作による状態数はそんなに多くない(log(n)の何倍か(?))．各値を動かした時にカウントして最後にカウントがn個となっている最小値を答える．動かすのは幅で見て，コストを足していった．

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
#include <queue>

#define REP(i,k,n) for(int i=k;i<n;i++)
#define rep(i,n) for(int i=0;i<n;i++)
#define INF 1<<30
#define pb push_back
#define mp make_pair

using namespace std;
typedef long long ll;
typedef pair<int,int> P;

int d[100005];
bool used[100005];
int cnt[100005];

int main() {
    int n;
    cin >> n;

    vector<int> v(n);
    rep(i,n) cin >> v[i];

    rep(i,100005) d[i] = INF;
    memset(cnt,0,sizeof(cnt));

    rep(i,n) {
        memset(used,0,sizeof(used));
        used[v[i]] = true;
        if(d[v[i]] == INF) d[v[i]] = 0;
        cnt[v[i]]++;

        queue<pair<int,int> > que;
        que.push(mp(v[i],0));

        while(que.size()) {
            pair<int,int> p = que.front();
            que.pop();

            int f = p.first;
            int cost = p.second;

            int t = f/2;
            int t2 = f*2;

            if(0 < t && t < 100005 && !used[t]) {
                used[t] = true;
                cnt[t]++;
                if(d[t] == INF) d[t] = cost+1;
                else d[t] += cost+1;
                que.push(make_pair(t,cost+1));
            }

            if(0 < t2 && t2 < 100005 && !used[t2]) {
                used[t2] = true;
                cnt[t2]++;
                if(d[t2] == INF) d[t2] = cost+1;
                else d[t2] += cost+1;
                que.push(make_pair(t2,cost+1));
            }
        }

    }

    int ans = INF;
    rep(i,100005) {
        if(cnt[i] == n) ans = min(ans,d[i]);
    }

    cout << ans << endl;

    return 0;
}
```
コンテスト中，最後のほうでこの解法を思いついた．t，t2やその範囲でバグらせてしまい，時間内に提出できなかった．非常に悔しい(実力が足りない)．  

