---
layout: post
title: "yukicoder No.17 2つの地点に泊まりたい"
date: 2015-09-03 13:26:09 +0900
comments: true
categories: [yukicoder,星2]
---

http://yukicoder.me/problems/61  

# 考察
nが50と小さいので，0とn-1以外の2つの地点(i,j)を列挙する．0~i，i~j，j~n-1の最短距離，i，jの滞在コストの和の最小値を求める．それぞれの地点からのdijkstraをやったが最初にwarshall_floydをしたほうがスマートに書けそう．

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

struct edge {
    int from,to;
    int cost;

    edge(int t,int c) : to(t),cost(c) {}
    edge(int f,int t,int c) : from(f),to(t),cost(c) {}

    bool operator<(const edge &e) const {
        return cost < e.cost;
    }
};

vector<edge> G[55];
int d[55];

void dijkstra(int s,int n) {
    priority_queue<P,vector<P>,greater<P> > que;
    fill(d,d+n,INF);

    d[s] = 0;
    que.push(P(0,s));

    while(que.size()) {
        P p = que.top();
        que.pop();

        int v = p.second;
        if(d[v] < p.first) continue;

        rep(i,G[v].size()) {
            edge e = G[v][i];
            if(d[e.to] > d[v] + e.cost) {
                d[e.to] = d[v] + e.cost;
                que.push(P(d[e.to],e.to));
            }
        }
    }
}

int main() {
    int n;
    cin >> n;

    vector<int> w(n);
    rep(i,n) cin >> w[i];

    int m;
    cin >> m;

    rep(i,m) {
        int a,b,c;
        cin >> a >> b >> c;

        G[a].push_back(edge(b,c));
        G[b].push_back(edge(a,c));
    }

    int ans = INF;
    REP(i,1,n-1) {
        REP(j,1,n-1) {
            if(i == j) continue;
            int cost = 0;

            dijkstra(0,n);
            if(d[i] == INF) continue;
            cost += d[i];

            dijkstra(i,n);
            if(d[j] == INF) continue;
            cost += d[j];

            dijkstra(j,n);
            if(d[n-1] == INF) continue;
            cost += d[n-1];

            cost += w[i] + w[j];

            ans = min(ans,cost);
        }
    }

    cout << ans << endl;

    return 0;
}
```
