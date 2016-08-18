---
layout: post
title: "Codeforces311-div2C Arthur and Table"
date: 2015-07-12 23:17:25 +0900
comments: true
categories: Codeforces
---

http://codeforces.com/contest/557/problem/C  
{% m %}N本の足を持つテーブルがあり，各足には長さl_{i}と取り除くコストd_{i}が与えられる．{% em %}
足の長さがバラバラなのでこれを安定状態にしたい．安定状態になるには足の最大の長さの本数が全体の本数の半分より多ければよい．その時の最小のコストを求める．

Sampleを考える．長さ1を1マスとし，文字がコストを表す．取り除いた足を赤色で表現する．

Sample1  
{% img /images/Codeforces311-div2/image1.png %}  
長さ5を取り除くほうがコストが安い  

Sample2  
{% img /images/Codeforces311-div2/image2.png %}  
既に安定状態である  

Sample3  
{% img /images/Codeforces311-div2/image3.png %}  
長さ2に揃える．長さ3は全て取り除き，個数が半分より多くなるように長さ1を1つ取り除く．

# 考察
どの長さで揃えるかを探索する．仮に揃える長さをLと決めた場合，全体のコストの和からLを引き，後は長さLの足の個数-1個分残すようにすればよいとわかる．長さでsortし小さい順から見ていけば，その処理ができる．

揃える長さを緑とした時に，赤の部分は全て取り除くことになる．
{% img /images/Codeforces311-div2/image4.png %}  
後は緑より小さい足を緑の個数より少なくすればよい．出来るだけコストを抑えたいのでコストが大きい棒を残すようにする．よって全体のコストの和-揃える長さ-(それより長さが短い足をコストの大きい順に個数が少なくなるまで)で求める．

# Code
コストが大きい順に見たいためpriority_queueを用いて大きい順に取った．最初は小さい順からとってしまい．WAを生やした．mapを使っているからidなどを作らずにmap巡回をしたほうがより分かりやすい(?)．

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
    int n;
    cin >> n;

    vector<int> L(n),D(n);
    rep(i,n) cin >> L[i];
    rep(i,n) cin >> D[i];

    vector<int> id(L.begin(),L.end());
    sort(id.begin(),id.end());
    id.erase(unique(id.begin(),id.end()),id.end());

    int sum = 0;
    map<int,vector<int> > m;
    rep(i,n) {
        m[L[i]].push_back(D[i]);
        sum += D[i];
    }
    
    int ans = sum;
    priority_queue<int> que;
    rep(i,id.size()) {
        vector<int> v(m[id[i]].begin(),m[id[i]].end());
        int res = sum;
        int cnt = v.size()-1;

        rep(j,v.size()) res -= v[j];

        vector<int> t;
        while(que.size() && cnt) {
            int q = que.top();
            que.pop();

            res -= q;
            t.push_back(q);
            cnt--;
        }

        ans = min(ans,res);

        rep(j,t.size()) que.push(t[j]);
        rep(j,v.size()) que.push(v[j]);
    }
    
    cout << ans << endl;

    return 0;
}
```
