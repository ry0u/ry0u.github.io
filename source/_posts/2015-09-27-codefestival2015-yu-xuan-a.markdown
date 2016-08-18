---
layout: post
title: "CodeFestival2015 予選A"
date: 2015-09-27 17:12:14 +0900
comments: true
categories: [CodeFestival2015]
---

# 振り返り

2分探索をバグらせる以前に，左に行って折り返すより，右に行ってから左に行く方がより効率がいいということに気づいていなかった．また2分探索の範囲も最初はrをn+1にしていたが，答えがnを超える時に対応できず，また{% m %} 1e+9 {% em %}でも足りず．  
考察が十分に出来てないし，実装も出来ずでダメダメだった．結果として，166位．予選Bを頑張るのみ．

# A - CODE FESTIVAL 2015

http://code-festival-2015-quala.contest.atcoder.jp/tasks/codefestival_2015_qualA_a  

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
    string s;
    cin >> s;

    int len = s.size();
    rep(i,len-4) {
        cout << s[i];
    }

    cout << "2015" << endl;

    return 0;
}
```

# B - とても長い文字列  

http://code-festival-2015-quala.contest.atcoder.jp/tasks/codefestival_2015_qualA_b  

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

    ll ans = 0;
    rep(i,n) {
        int x;
        cin >> x;

        ans = ans * 2 + x;
    }

    cout << ans << endl;

    return 0;
}
```

# C - 8月31日

http://code-festival-2015-quala.contest.atcoder.jp/tasks/codefestival_2015_qualA_c  

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
    int n,t;
    cin >> n >> t;

    vector<int> a(n), b(n);
    vector<int> v(n);
    ll sum = 0;
    rep(i,n) {
        cin >> a[i] >> b[i];
        sum += a[i];
        v[i] = a[i] - b[i];
    }

    sort(v.begin(), v.end(), greater<int>());

    if(sum <= t){
        cout << 0 << endl;
        return 0;
    }

    bool flag = false;
    rep(i,n) {
        sum -= v[i];

        if(sum <= t) {
            flag = true;
            cout << i+1 << endl;
            break;
        }
    }

    if(!flag) cout << -1 << endl;
    return 0;
}
```

# D - 壊れた電車

http://code-festival-2015-quala.contest.atcoder.jp/tasks/codefestival_2015_qualA_d  

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
    ll n, m;
    cin >> n >> m;

    vector<ll> v;
    rep(i,m) {
        ll x;
        cin >> x;

        v.push_back(x);
    }

    if(n == m) {
        cout << 0 << endl;
        return 0;
    }

    ll l = 0, r = 5000000000;
    while(r-l > 1) {
        ll mid = (l+r) / 2;
    
        bool flag = true;
    
        ll now = 0;
        ll id = 0;
       
        while(id != m) {
            ll cnt = mid;

            if(now >= v[id]) {
                now = max(now, v[id] + cnt);
                id++;
                continue;
            }
       
            ll d = v[id] - now - 1;

            if(cnt >= d) {
                now = v[id] + max( cnt - d*2 , (cnt-d) / 2);
            } else {
                flag = false;
                break;
            }

            id++;
        }
    
        ll cur = n + 1;
        id = m-1;
        bool ch = true;
       
        while(id != -1) {
            ll cnt = mid;
    
            if(cur <= v[id]) {
                cur = min(cur, v[id] - cnt);
                id--;
                continue;
            }
       
            ll d = cur - v[id] - 1;

            if(cnt >= d*2) {
                cur = v[id] - min(cnt - d*2, (cnt-d) / 2);
            } else {
                ch = false;
                break;
            }

            id--;
        }
    
        if(flag && now >= n) {
            r = mid;
        } else if(ch && cur <= 1) {
            r = mid;
        }else {
            l = mid;
        }
    }

    cout << r << endl;
    return 0;
}
```

