---
layout: post
title: "AOJ1327 One-Dimensional Cellular Automaton"
date: 2016-03-24 01:32:29 +0900
comments: true
categories: [AOJ-ICPC, 300, 行列累乗]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-type="article" data-card-branding="0"><h4><a href="http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=1327">One-Dimensional Cellular Automaton</a></h4><p>There is a one-dimensional cellular automaton consisting of cells. Cells are numbered from 0 to N − 1. Each cell has a state represented as a non-negative integer less than M. The states of cells evolve through discrete time steps. We denote the state of the i-th cell at time t as S( i, t).</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

{% math %}
	S(i, t+1) = A \cdot S(i-1, t) + B \cdot S(i, t) + C \cdots S(i+1, t) {\rm mod} M
{% endmath %}
をそのままやると {% m %} O(NT) {% em %}となり間に合わない．変換行列を

{% math %}
	\left(
		\begin{array}{ccccc}
			b & c & 0 & 0 & 0 \\
			a & b & c & 0 & 0 \\
			0 & a & b & c & 0 \\
			0 & 0 & a & b & c \\
			0 & 0 & 0 & a & b \\
		\end{array}
	\right) 
{% endmath %}

とする．これの行列の{% m %} (i, j) {% em %}が{% m %} S(j, 0) {% em %}の係数となる．よって

{% math %}
	\left(
		\begin{array}{ccccc}
			b & c & 0 & 0 & 0 \\
			a & b & c & 0 & 0 \\
			0 & a & b & c & 0 \\
			0 & 0 & a & b & c \\
			0 & 0 & 0 & a & b \\
		\end{array}
	\right) ^T
	\left(
		\begin{array}{c}
			S(0, 0) \\
			S(1, 0) \\
			S(2, 0) \\
			S(3, 0) \\
			S(4, 0)
		\end{array}
	\right)
	
{% endmath %}

この変換行列の {% m %} T {% em %}乗は {% m %} logT {% em %}で出来るので {% m %} O(N logT) {% em %}となり間に合う．  
行列累乗の問題を初めて解けた(文字のまま展開していたら気付いた)．非常に嬉しい．

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

int MOD;

struct Mat {
	vector<vector<ll> > dat;
	int n;

	Mat(int n) : n(n), dat(n, vector<ll>(n)) {}

	Mat(vector<vector<ll> > dat) : n(dat.size()), dat(dat) {}

	Mat I(int n) {
		Mat ret(n);
		rep(i, n) ret.dat[i][i] = 1;
		return ret;
	}

	Mat mul(Mat &b) {
        Mat ret(n);
		rep(i, n) rep(j, n) rep(k, n) (ret.dat[i][j] += dat[i][k] * b.dat[k][j]) %= MOD;
        return ret;
	}

	Mat pow(ll b) {
		Mat ret = I(n);
        for (Mat A = *this; b > 0; A = A.mul(A) , b /= 2) if (b & 1) ret = A.mul(ret);
        return ret;
	}
};

int main() {
	int n, m, a, b, c, t;
	while(cin >> n >> m >> a >> b >> c >> t) {
		if(n == 0 && m == 0 && a == 0 && b == 0 && c == 0 && t == 0) break;

		MOD = m;

		vector<int> s(n);
		rep(i, n) cin >> s[i];

		int s1 = -1, s2 = 0, s3 = 1;
		Mat mat(n);
		rep(i, n) {
			if(s1 >= 0) {
				mat.dat[i][s1] = a;
			}
			mat.dat[i][s2] = b;
			if(s3 < n) {
				mat.dat[i][s3] = c;
			}
			
			s1++; s2++; s3++;
		}

		Mat ret = mat.pow(t);

		rep(i, n) {
			ll sum = 0;
			rep(j, n) {
				sum += s[j] * ret.dat[i][j];
				sum %= MOD;
			}

			cout << sum;

			if(i == n-1) cout << endl;
			else cout << " ";
		}
	}

	return 0;
}
```

