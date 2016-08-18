---
layout: post
title: "yukicoder No.43 野球の試合"
date: 2015-09-03 13:39:56 +0900
comments: true
categories: [yukicoder,星2,全探索]
---

http://yukicoder.me/problems/8

# 考察
0番目のチームに"-"がある場合は必ず，勝ちにする．それ以外の場合はより勝っているチームが負け，負けているチームが勝つようにする，という貪欲は上手くいきそうにない．Nが最大6なので，全てが"-"だとしても状態数は2^18．よって全ての状態を列挙して，最大で何位になれるかを見る．

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
    int n;
    cin >> n;

    vector<string> v(n);
    rep(i,n) cin >> v[i];

    vector<int> cnt(n);

    rep(i,n) {
        rep(j,n) {
            if(v[i][j] == '#') continue;
            if(v[i][j] == 'o') cnt[i]++;
        }
    }

    set<P> st;
    rep(i,n) {
        rep(j,n) {
            if(v[i][j] == '-') {
                int s = i, t = j;
                if(s > t) swap(s,t);

                st.insert(P(s,t));
            }
        }
    }

    int ans = n;
    int m = st.size();
    if(m == 0) {
        int val = cnt[0];

        sort(cnt.begin(),cnt.end());
        cnt.erase(unique(cnt.begin(),cnt.end()),cnt.end());
        reverse(cnt.begin(),cnt.end());

        rep(i,cnt.size()) {
            if(val == cnt[i]) {
                ans = i;
                break;
            }
        }
    } else {

        vector<P> v2(st.begin(), st.end());

        rep(i,1<<m) {
            vector<int> res(cnt.begin(), cnt.end());
            rep(j,m) {
                P p = v2[j];
                if(i & (1<<j)) {
                    res[p.first]++;
                } else {
                    res[p.second]++;
                }
            }

            int val = res[0];
            sort(res.begin(),res.end());
            res.erase(unique(res.begin(),res.end()),res.end());
            reverse(res.begin(),res.end());

            int id = 0;
            rep(j,res.size()) {
                if(res[j] == val) {
                    id = j;
                    break;
                }
            }

            ans = min(ans,id);
        }
    }

    cout << ans + 1 << endl;

    return 0;
}
```

