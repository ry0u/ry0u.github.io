---
layout: post
title: "ABC010C 浮気調査"
date: 2016-04-03 17:05:27 +0900
comments: true
categories: [ABC, AtCoder, 幾何]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-branding="0" data-card-type="article-full"><h4><a href="http://abc010.contest.atcoder.jp/tasks/abc010_3">C: 浮気調査 - AtCoder Beginner Contest 010 | AtCoder</a></h4><p>(null)</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

一人ひとり，その場所に行って電話に出た場所まで {% m %} T {% em %}以内にいけるを試す．移動した距離が {% m %} T * V {% em %}より小さければ可能である．

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
#include <cmath>

#define REP(i,k,n) for(int i=k;i<n;i++)
#define rep(i,n) for(int i=0;i<n;i++)
#define INF 1<<30
#define pb push_back
#define mp make_pair
#define EPS 1e-8
#define equals(a,b) fabs((a) - (b)) < EPS

using namespace std;
typedef long long ll;
typedef pair<int,int> P;

struct Point {
	double x, y;

	Point(double x=0, double y=0) : x(x), y(y) {}

	Point operator+(const Point &o) const { return Point(x+o.x, y+o.y); }

	Point operator-(const Point &o) const { return Point(x-o.x, y-o.y); }

	Point operator*(const double m) const { return Point(x*m, y*m); }

	Point operator/(const double d) const { return Point(x/d, y/d); }

	bool operator<(const Point &o) const { return x != o.x ? x < o.x : y < o.y; }

	bool operator==(const Point &o) const { return fabs(x-o.x) < EPS && fabs(y-o.y) < EPS; }
};

ostream& operator << (ostream& os, const Point& p) {
	os << "(" << p.x << ", " << p.y << ")";
	return os;
}

double dot(Point a, Point b) { return a.x * b.x + a.y * b.y; }
double cross(Point a, Point b) { return a.x * b.y - a.y * b.x; }
double atan(Point p) { return atan2(p.y, p.x); }
double norm(Point p) { return p.x * p.x + p.y * p.y; }
double abs(Point p) { return sqrt(norm(p)); }
double distancePP(Point p, Point o) { return sqrt(norm(o - p)); }

int main() {
	Point s, t;
	int T, V;
	cin >> s.x >> s.y >> t.x >> t.y >> T >> V;

	bool flag = false;

	int n;
	cin >> n;

	rep(i, n) {
		Point p;
		cin >> p.x >> p.y;

		if(distancePP(s, p) + distancePP(p, t)  <= T * V) {
			flag = true;
		}
	}

	if(flag) {
		cout << "YES" << endl;
	} else {
		cout << "NO" << endl;
	}

	return 0;
}
```

