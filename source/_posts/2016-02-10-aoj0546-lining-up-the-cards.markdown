---
layout: post
title: "AOJ0546 Lining up the cards"
date: 2016-02-10 21:50:42 +0900
comments: true
categories: [AOJ, 全探索]
---

問題文  
http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=0546

<!-- more-->
最大{% m %} Nが10{% em %}なので全探索してsetにぶっ込んで終了だと思ったけどTLEした．k番目より後ろが変更してもできる整数は一緒なので，k番目までの状態もsetにぶっ込んだらACした．

# Code

```cpp
#include <iostream>
#include <sstream>
#include <string>
#include <vector>
#include <algorithm>
#include <set>
#include <map>
 
#define REP(i,k,n) for(int i=0;i<n;i++)
#define rep(i,n) for(int i=0;i<n;i++)
 
using namespace std;
 
typedef long long ll;
typedef pair<int, int> P;
 
int func(vector<int> v, int k) {
    stringstream ss;
    rep(i, k) {
        ss << v[i];
    }
 
    int ret;
    ss >> ret;
 
    return ret;
}
 
int main() {
    int n;
    while(cin >> n) {
        if(n == 0) break;
 
        int k;
        cin >> k;
         
        vector<int> v(n);
        rep(i, n) cin >> v[i];
 
        sort(v.begin(), v.end());
 
        set<int> ans;
        set<vector<int> > st;
 
 
        do {
            vector<int> ret(k);
            rep(i, k)  ret[i] = v[i];
 
            if(st.find(ret) == st.end()) {
                st.insert(ret);
                ans.insert(func(v, k));
            } else continue;
        }while(next_permutation(v.begin(), v.end()));
 
        cout << ans.size() << endl;
    }
    return 0;
}
```
