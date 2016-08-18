---
layout: post
title: "SRM502 TheLotteryBothDivs"
date: 2015-07-17 19:44:40 +0900
comments: true
categories: [topcoder,srm]
---

000000000~999999999の10^9の中で宝くじが当たる確率を求める．また下の桁が一致していれば当たりとなる．

# 考察
まず同じ数が与えられる可能性もあるので，setに投げて重複を消してから，下の桁が一致している数を消す．桁数が小さいほうが含まれるものが多いので，桁数が大きい方を消す．
後は0.1^(桁数)のsumが答えとなる．

# Code

```cpp
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>
#include <sstream>
#include <cstring>
#include <queue>
#include <set>
#include <map>

#define REP(i,k,n) for(int i=k;i<n;i++)
#define rep(i,n) for(int i=0;i<n;i++)

using namespace std;
typedef long long ll;

class TheLotteryBothDivs {
	public:
	double find(vector <string> goodSuffixes) {
        set<string> st;
        rep(i,goodSuffixes.size()) {
            st.insert(goodSuffixes[i]);
        }

        vector<string> v;
        set<string>::iterator ite;
        for(ite = st.begin();ite != st.end(); ite++) {
            v.push_back(*ite);
        }
        
        vector<string> res;
        rep(i,v.size()) {
            bool flag = false;
            rep(j,v.size()) {
                if(v[i].size() > v[j].size()) {
                    bool ch = true;
                    rep(k,v[j].size()) {
                        if(v[i][v[i].size()-1-k] == v[j][v[j].size()-1-k]) continue;
                        ch = false;
                    }

                    if(ch) flag = true;
                }
            }
            if(!flag) res.push_back(v[i]);
        }

        double ans = 0;
        rep(i,res.size()) {
            double t = 1.0;

            rep(j,res[i].size()) {
                t *= 0.1;
            }

            ans += t;
        }
        return ans;
	}
};
```

