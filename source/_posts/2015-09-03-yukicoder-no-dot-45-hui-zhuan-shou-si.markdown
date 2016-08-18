---
layout: post
title: "yukicoder No.45 回転寿司"
date: 2015-09-03 14:00:37 +0900
comments: true
categories: [yukicoder,星2,動的計画法]
---

http://yukicoder.me/problems/78  

# 考察
動的計画法で解く．\[i\]\[0\]をi番目を取らない，\[i\]\[1\]をi番目を取るとして，dp\[i\]\[j\]をi番目までの最大値とする．dp\[i\]\[0\] = max(i-1番目を取った，i-2番目を取った)，dp\[i\]\[1\] = max(i-1番目を取らない+i番目の価値，i-2番目を取った+i番目の価値)で求める．

{% math %}
\begin{eqnarray}
\begin{cases}
dp[0][1] = v[0] \\
dp[1][0] = v[0] \\
dp[1][1] = v[1] \\
dp[i][0] = max(dp[i-1][1],dp[i-2][1]) (i \geq 2) \\
dp[i][1] = max(dp[i-1][0]+v[i],dp[i-2][1]+v[i]) (i \geq 2)
\end{cases}
\end{eqnarray}
{% endmath %}

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

    vector<int> v(n);
    rep(i,n) cin >> v[i];

    int dp[1005][2];
    memset(dp,0,sizeof(dp));

    dp[0][1] = v[0];
    dp[1][0] = v[0];
    dp[1][1] = v[1];

    REP(i,2,n) {
        dp[i][0] = max(dp[i-1][1],dp[i-2][1]);
        dp[i][1] = max(dp[i-1][0] + v[i],dp[i-2][1] + v[i]);
    }

    cout << max(dp[n-1][0],dp[n-1][1]) << endl;

    return 0;
}
```

