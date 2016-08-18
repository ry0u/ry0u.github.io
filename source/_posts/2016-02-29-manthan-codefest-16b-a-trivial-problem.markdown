---
layout: post
title: "Manthan, Codefest 16B A Trivial Problem"
date: 2016-02-29 14:23:55 +0900
comments: true
categories: [Codeforces]
---
<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-chrome="0" data-card-controls="0"><h4><a href="http://codeforces.com/contest/633/problem/B">Problem - B - Codeforces</a></h4><p>Mr. Santa asks all the great programmers of the world to solve a trivial problem. He gives them an integer m and asks for the number of positive integers n, such that the factorial of n ends with exactly m zeroes. Are you among those great programmers who can solve this problem?</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

階乗の末尾に{% m %} 0がm {% em %}個ある数は何個あるか．  
末尾に{% m %} 0 {% em %}が増えるのは{% m %} 10 {% em %}などを通るときである．{% m %} 10 = 2 \cdot 5 {% em %}で，{% m %} 2は5 {% em %}の数より圧倒的に多いので{% m %} 5 {% em %}を通るたびに0が増えると考える．{% m %} cnt {% em %}を末尾の{% m %} 0 {% em %}の個数とし，
{% math %}
	5の時は+1 \\
	25の時は+2 \\
	125の時は+3
{% endmath %}
のように5を因数としてもつ分だけ増やす．


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

	vector<int> v;
	int cnt = 0;

	rep(i, 5*100000) {
		if(i % 5 == 0) {
			int t = i;
			while(t != 0 &&t % 5 == 0) {
				t /= 5;
				cnt++;
			}
		}

		if(cnt == n) {
			v.push_back(i);
		} else if(cnt > n) {
			break;
		}
	}

	cout << v.size() << endl;
	rep(i, v.size()) {
		cout << v[i];
		if(i == v.size()-1) cout << endl;
		else cout << " ";
	}

	return 0;
}
```
