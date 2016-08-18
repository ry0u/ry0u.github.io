---
layout: post
title: "SRM500 D2H GeometricProgressions"
date: 2015-07-14 14:45:12 +0900
comments: true
categories: [topcoder,SRM]
---
2つの等比数列が与えられる．この中で異なる数の個数を求める

# 考察

愚直に計算するとオーバーフローする．ここで素因数分解を考える．いつも素因数分解というと
{% math %}
S = p_{1} p_{2} ... p_{r}
{% endmath %}
としていたが，(このままではやったらTLEになるので)
{% math %}
S = p_{1}^{e_{1}} p_{2}^{e_{2}} ... p_{r}^{e_{r}}
{% endmath %}
とした．

素因数分解は必ず一意に定まるので数が等しければ素因数分解も等しい．後はこれをmapに突っ込んでsizeを取った．  
SystemTest落ちまくり．以下の事を全く頭に入れていなかった．  
- 初項が0なら必ず0  
- 公比が1の場合は，必ず初項  
- 初項が0でなく公比が0の場合は，n == 1ならば0^0 = 1より初項だけ，n != 1ならば初項と0になる．

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

vector<int> func(int n) {
    vector<int> ret;
    if(n == 0 || n == 1) {
        ret.push_back(n);
        return ret;
    }

    int a = 2;
    while(n >= a*a) {
        if(n%a == 0) {
            n /= a;
            ret.push_back(a);
        }
        else {
            a++;
        }
    }

    if(n > 1) ret.push_back(n);

    return ret;
}

class GeometricProgressions {
	public:
	int count(int b1, int q1, int n1, int b2, int q2, int n2) {
        map<vector<pair<int,int> >,int> m;

        vector<int> B1 = func(b1);
        vector<int> Q1 = func(q1);
        vector<int> B2 = func(b2);
        vector<int> Q2 = func(q2);

        map<int,int> cnt;
        rep(i,B1.size()) {
            cnt[B1[i]]++;
        }

        vector<pair<int,int> > v;
        map<int,int>::iterator ite;
        for(ite = cnt.begin();ite != cnt.end();ite++) {
            v.push_back(make_pair(ite->first,ite->second));
        }

        m[v]++;

        if(q1 == 0) {
            if(b1 != 0 && n1 != 1) {
                vector<pair<int,int> > res;
                res.push_back(make_pair(0,1));
                m[res]++;
            }
        }
        else if(b1 != 0 && q1 != 1) {
            rep(i,n1-1) {
                rep(j,Q1.size()) cnt[Q1[j]]++;
        
                vector<pair<int,int> > res;
                map<int,int>::iterator ite;
                for(ite = cnt.begin();ite != cnt.end();ite++) {
                    res.push_back(make_pair(ite->first,ite->second));
                }
                m[res]++;
            }
        }

        map<int,int> cnt2;
        rep(i,B2.size()) {
            cnt2[B2[i]]++;
        }

        vector<pair<int,int> > v2;
        for(ite = cnt2.begin();ite != cnt2.end();ite++) {
            v2.push_back(make_pair(ite->first,ite->second));
        }

        m[v2]++;

        if(q2 == 0) {
            if(b2 != 0 && n2 != 1) {
                vector<pair<int,int> > res;
                res.push_back(make_pair(0,1));
                m[res]++;
            }
        }
        else if(b2 != 0 && q2 != 1) {
            rep(i,n2-1) {
                rep(j,Q2.size()) cnt2[Q2[j]]++;

                vector<pair<int,int> > res;
                map<int,int>::iterator ite;
                for(ite = cnt2.begin();ite != cnt2.end();ite++) {
                    res.push_back(make_pair(ite->first,ite->second));
                }

                m[res]++;
            }
        }

        return m.size();
	}
};
```

場合分けの書き方がやばい．もっとシンプルに書けるようになりたい．
