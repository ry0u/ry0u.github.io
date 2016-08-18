---
layout: post
title: "Codeforces322-div2C Developing Skills"
date: 2015-09-30 15:41:28 +0900
comments: true
categories: [Codeforces, gready]
---

http://codeforces.com/contest/581/problem/C  

主人公は，{% m %}n{% em %} 個のskillを持っている．{% m %} \lfloor \frac{a_{i}}{10} \rfloor {% em %}が価値になる．killの値を{% m %} k {% em %}だけ上げることが出来る時に，最大の価値の合計を求めよ．

# 考察
10刻みで1上がることが分かるので，次に価値が上がるまでいくら上げればいいかをpriority_queueに突っ込む．そしてこの値が小さい順にkを使って最大を計算する．それぞれのskillが100を超えてはいけないことに注意する（落ちた）．

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

int main() {
    int n,k;
    cin >> n >> k;

    vector<int> v(n);
    rep(i,n) {
        cin >> v[i];
    }


    priority_queue<P, vector<P>, greater<P> > que;

    rep(i,n) {
        int d = v[i] % 10;
        que.push(P(10 - d,v[i]));
    }

    int ans = 0;
    while(k > 0 && que.size()) {
        P p = que.top();
        que.pop();

        if(p.second == 100) {
            ans += 10;
            continue;
        }

        if(k >= p.first) {
            que.push(P(10,p.second + p.first));
            k -= p.first;
        } else {
            p.first -= k;
            que.push(P(p.first, p.second+k));
            k = 0;
        }
    }

    while(que.size()) {
        P p = que.top();
        que.pop();

        ans += p.second / 10;
    }

    cout << ans << endl;

    return 0;
}

```
