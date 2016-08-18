---
layout: post
title: "yukicoder No.120 傾向と対策:門松列(その1)"
date: 2015-09-03 22:07:12 +0900
comments: true
categories: [yukicoder,星2]
---

http://yukicoder.me/problems/291  

# 考察
自由に並び替えて良いため，関係あるのは長さの竹が何個あるか．sortして小さい順にとるような貪欲では例えば
{% math %}
\{1,1,2,3,3,3,3,3,4,4,4\}
{% endmath %}
の時に，{1,2,3}とペアを作ってしまうとペアが2つしか作れない．長さが違えばよいで，個数が多い順にとっていく貪欲で上手くいった．多い順に取るのはpriority_queueを使った．

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
    int t;
    cin >> t;

    rep(q,t) {
        int n;
        cin >> n;

        vector<int> v(n);
        map<int,int> m;
        rep(i,n) {
            cin >> v[i];
            m[v[i]]++;
        }

        priority_queue<int> que;
        map<int,int>::iterator ite;
        for(ite = m.begin(); ite != m.end(); ite++) {
            que.push(ite->second);
        }

        int cnt = 0;
        while(que.size() > 2) {
            cnt++;
            vector<int> t(3);

            rep(i,3) {
                t[i] = que.top();
                que.pop();
            }

            rep(i,3) {
                if(t[i] == 1) continue;
                que.push(t[i]-1);
            }
        }

        cout << cnt << endl;

    }
    return 0;
}
```

