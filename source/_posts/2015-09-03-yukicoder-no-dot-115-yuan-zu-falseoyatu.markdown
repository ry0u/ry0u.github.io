---
layout: post
title: "yukicoder No.115 遠足のおやつ"
date: 2015-09-03 21:51:08 +0900
comments: true
categories: [yukicoder,星2]
---

http://yukicoder.me/problems/205  

# 考察
買うK個の最小値と最大値を出す．
{% math %}
down[] = \{1,2,...,k\}  \\
up[] = \{n-k+1,...,n-1,n\}
{% endmath %}
としてこの中に収まるようにする．辞書順最小にしたいので，左側からdownに近づけていき，その分を右側に持っていく．この操作を可能な限り行い，変化がなくなったら終了．

# Code

```cpp
#include <iostream>
#include <vector>
#include <string>
#include <cstring>
#include <algorithm>
#include <sstream>
#include <cassert>
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
    int n,d,k;
    cin >> n >> d >> k;

    int sum = 0, t = n;
    rep(i,k) {
        sum += t;
        t--;
    }

    if(d < (k*(k+1))/2 || d > sum) cout << -1 << endl;
    else {
        vector<int> v;
        rep(i,k) v.push_back(d/k);

        int cnt = d % k;
        rep(i,cnt) {
            v[i]++;
        }

        sort(v.begin(),v.end());

        vector<int> down(n), up(n);
        rep(i,n) {
            down[i] = i+1;
            up[i] = n-k+1+i;
        }

        while(true) {
            vector<int> temp(v.begin(),v.end());
            int s = 0, t = v.size()-1;

            while(true) {
                if(s == t) break;

                if(v[s] == down[s] || v[s] == up[s]) {
                    s++;
                    continue;
                }

                if(v[t] == up[t]) {
                    t--;
                    continue;
                }


                int diff;
                if(v[s] >= up[s]) {
                    diff = v[s] - up[s];
                } else {
                    diff = v[s] - down[s];
                }

                while(diff) {
                    if(v[t] + diff <= up[t]) {
                        v[s] -= diff;
                        v[t] += diff;
                        diff = 0;
                    }
                    else {
                        v[s] -= up[t] - v[t];
                        diff -= up[t] - v[t];
                        v[t] = up[t];
                        t--;
                    }

                    if(s == t) break;
                }

            }

            sort(v.begin(),v.end());

            bool flag = true;
            rep(i,k) {
                if(v[i] != temp[i]) flag = false;
            }

            if(flag) break;
        }

        rep(i,v.size()) {
            cout << v[i];

            if(i == v.size()-1) cout << endl;
            else cout << " ";
        }
    }

    return 0;
}
```

