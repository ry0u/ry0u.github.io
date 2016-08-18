---
layout: post
title: "Codeforces352-div2C Recycling Bottles"
date: 2016-05-12 14:55:05 +0900
comments: true
categories: [Codeforces, 幾何]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-branding="0" data-card-type="article"><h4><a href="http://codeforces.com/contest/672/problem/C">Problem - C - Codeforces</a></h4><p>It was recycling day in Kekoland. To celebrate it Adil and Bera went to Central Perk where they can take bottles from the ground and put them into a recycling bin. We can think Central Perk as coordinate plane. There are n bottles on the ground, the i-th bottle is located at position .</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

荷物は一個しか持てないので，毎回拾って戻して，拾って戻してを繰り返す．なので，まずそれぞれから {% m %} (tx, ty) {% em %}から行く道と帰る道を両方張る．それから {% m %} a {% em %}が最初に拾った方が良いものがあれば拾う． {% m %} b {% em %}が最初に拾った方が良いものがあれば拾う．とする  
また，{% m %} a {% em %}が動かず {% m %} b {% em %}が全部拾う場合や， {% m %} a {% em %}が動かず {% m %} b {% em %}が拾う場合がある．最初に拾う点を選んでむしろ距離が増えてしまった場合は，その場に立ち止まるようにする．  
また距離の初期値のINFがオーバーフローした．制約に気を付けたい．

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
#define INF 1LL<<60
#define pb push_back
#define mp make_pair
#define EPS 1e-8

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
	Point a, b, t;
	cin >> a.x >> a.y >> b.x >> b.y >> t.x >> t.y;

	int n;
	cin >> n;

	vector<Point> v(n);
	rep(i, n) cin >> v[i].x >> v[i].y;

	double sum = 0;
	rep(i, n) {
		sum += distancePP(t, v[i]) * 2;
	}

	double A = sum, B = sum;
	{
		double len = -INF;
		int id = -1;
		rep(i, n) {
			double p = distancePP(t, v[i]);
			double q = distancePP(a, v[i]);

			if(p - q > len) {
				len = p - q;
				id = i;
			}
		}

		double len2 = 0;
		int id2 = -1;
		rep(i, n) {
			if(i == id) continue;

			double p = distancePP(t, v[i]);
			double q = distancePP(b, v[i]);

			if(p - q > len2) {
				len2 = p - q;
				id2 = i;
			}
		}

		// cout <<  " A -> B" << endl;
		// cout << id << " " << id2 << endl;
		// cout << "sum:" << sum << endl;
		// cout << distancePP(t, v[id]) << " " << distancePP(a, v[id]) << " " << len << endl;
		// cout << distancePP(t, v[id2]) << " " << distancePP(b, v[id2]) << " " << len2 << endl;

		if(id != -1) A -= len;
		if(id2 != -1) A -= len2;
	}
	{
		double len = -INF;
		int id = -1;

		rep(i, n) {
			double p = distancePP(t, v[i]);
			double q = distancePP(b, v[i]);

			if(p - q > len) {
				len = p - q;
				id = i;
			}
		}

		double len2 = 0;
		int id2 = -1;
		rep(i, n) {
			if(i == id) continue;

			double p = distancePP(t, v[i]);
			double q = distancePP(a, v[i]);

			if(p - q > len2) {
				len2 = p - q;
				id2 = i;
			}
		}

		// cout <<  " B -> A" << endl;
		// cout << id << " " << id2 << endl;
		// cout << "sum:" << sum << endl;
		// cout << distancePP(t, v[id]) << " " << distancePP(b, v[id]) << " " << len << endl;
		// cout << distancePP(t, v[id2]) << " " << distancePP(a, v[id2]) << " " << len2 << endl;

		if(id != -1) B -= len;
		if(id2 != -1) B -= len2;
	}

	cout << fixed;
	cout.precision(20);
	cout << min(A, B) << endl;

	return 0;
}
```

