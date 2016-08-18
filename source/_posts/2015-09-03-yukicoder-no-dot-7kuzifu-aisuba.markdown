---
layout: post
title: "yukicoder No.78 クジ付アイスバー"
date: 2015-09-03 21:15:58 +0900
comments: true
categories: [yukicoder,星2]
---

http://yukicoder.me/problems/152  

# 考察
以下の通りに場合分けして考えた．  
- n > kの時，食べる予定のアイスバーの分回せばいい(最大50)．  
- そうではない時，まず1箱分回した時に，いくつ当たりが残るかを調べる．  
- ここで残った当たりが1箱勝った数と同じ場合はそれが答え(目標のkまで最初の1箱分だけでいける)  
- 同じでなかった時は，全部開ける箱と，買い終わってアイスが残る箱に分けて考える

まずは愚直にやってTLEをして，--と++を間違えてWAを生やした

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
    int n,k;
    cin >> n >> k;
    string s;
    cin >> s;

    vector<int> v(n);
    rep(i,n) v[i] = s[i] - '0';

    if(n > k) {
        int ans = 0, cnt = 0;
        rep(i,k) {
            if(cnt > 0) cnt--;
            else ans++;

            cnt += v[i];
        }

        cout << ans << endl;
    } else {
        int res = 0, cnt = 0;
        rep(i,n) {
            if(cnt > 0) cnt--;
            else res++;

            cnt += v[i];
        }

        if(cnt >= res) {
            cout << res << endl;
        } else {
            int m = 0;
            int ans = 0, q = 0;
            while(m + n <= k) {
                m += n;
                if(q >= res) {
                    q -= res;
                    q += cnt;
                } else {
                    ans += res - q;
                    q = cnt;
                }
            }

            rep(i,k%n) {
                if(q > 0) q--;
                else ans++;

                q += v[i];
            }

            cout << ans << endl;
        }

    }
    return 0;
}
```

