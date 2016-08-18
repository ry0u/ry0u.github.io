---
layout: post
title: "yukicoder No.44 DPなすごろく"
date: 2015-09-03 13:53:47 +0900
comments: true
categories: [yukicoder,星2,動的計画法]
---

http://yukicoder.me/problems/76  

# 考察
1または2前に進むことが出来るので，i+2番目にマスに訪れるにはi番目のマス，またはi+1番目のマスから来るしかない．よって

{% math %}
\begin{eqnarray}
\begin{cases}
dp[0] = 1 \\
dp[1] = 1 \\
dp[i+2] = dp[i+1] + dp[i] (i \geq 2)
\end{cases}
\end{eqnarray}
{% endmath %}

が成り立つ．フィボナッチ階段を同じ考えである．

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

    vector<ll> dp(55);
    dp[0] = 1;
    dp[1] = 1;

    rep(i,n-1) {
        dp[i+2] = dp[i] + dp[i+1];
    }

    cout << dp[n] << endl;

    return 0;
}
```
