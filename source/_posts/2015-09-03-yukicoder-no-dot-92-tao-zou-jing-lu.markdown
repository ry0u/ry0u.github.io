---
layout: post
title: "yukicoder No.92 逃走経路"
date: 2015-09-03 21:40:54 +0900
comments: true
categories: [yukicoder,星2,グラフ]
---

http://yukicoder.me/problems/149  

# 考察
dfsで行けるとこ全部行ったらTLEした．戻ることも考えられるので，状態数がすごいことになることがわかっていなかった．よくよく考えると，i回目の移動でいる場所としてはmaxでNなので，行ける場所をもって最後まで残った場所を答える．

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

struct edge {
    int from,to;
    int cost;

    edge(int t,int c) : to(t),cost(c) {}
    edge(int f,int t,int c) : from(f),to(t),cost(c) {}

    bool operator<(const edge &e) const {
        return cost < e.cost;
    }
};

vector<int> v;
int n,m,k;

int main() {
    cin >> n >> m >> k;

    map<int,vector<edge> > edges;
    rep(i,m) {
        int a,b,c;
        cin >> a >> b >> c;

        a--;
        b--;

        edges[c].push_back(edge(a,b,c));
        edges[c].push_back(edge(b,a,c));
    }

    v.resize(k);
    rep(i,k) cin >> v[i];

    vector<int> ans(n);
    rep(i,n) ans[i] = i;
    rep(i,k) {
        bool used[105];
        memset(used,0,sizeof(used));

        rep(j,ans.size()) {
            used[ans[j]] = true;
        }

        vector<edge> es = edges[v[i]];
        vector<int> temp;
        rep(j,es.size()) {
            int f = es[j].from;
            int t = es[j].to;

            if(used[f]) {
                temp.push_back(t);
            }
        }

        sort(temp.begin(),temp.end());
        temp.erase(unique(temp.begin(),temp.end()),temp.end());

        ans.resize(temp.size());
        rep(j,temp.size()) ans[j] = temp[j];
    }

    sort(ans.begin(),ans.end());
    cout << ans.size() << endl;
    rep(i,ans.size()) {
        cout << ans[i] + 1;
        if(i == ans.size()-1) cout << endl;
        else cout << " ";
    }

    return 0;
}
```
