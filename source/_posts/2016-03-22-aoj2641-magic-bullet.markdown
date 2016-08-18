---
layout: post
title: "AOJ2641 Magic Bullet"
date: 2016-03-22 21:29:19 +0900
comments: true
categories: [AOJ-ICPC, 250, 幾何]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-type="article" data-card-branding="0"><h4><a href="http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=2641">Magic Bullet | Aizu Online Judge</a></h4><p>Introduction to Programming Introduction to Algorithms and Data Structures Library of Data Structures Library of Graph Algorithms Library of Computational Geometry Library of Dynamic Programming Library of Number Theory</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

点{% m %} (sx, sy, sz) {% em %}，点 {% m %} (dx, dy, dz) {% em %}を結ぶ線分が{% m %} N {% em %}個の球と交差しているかを見る．それぞれの球の中心から直線への射影を出す．その点が線分に収まっていて，球の中心との距離がr以内なら交差しているとした．

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

struct Point3D {
	double x, y, z;

	Point3D() : x(0), y(0), z(0) {}

	Point3D(double x, double y, double z) : x(x), y(y), z(z) {}

	Point3D operator+(const Point3D &o) const { return Point3D(x+o.x, y+o.y, z+o.z); }

	Point3D operator-(const Point3D &o) const { return Point3D(x-o.x, y-o.y, z-o.z); }

	Point3D operator*(const double m) const { return Point3D(x*m, y*m, z*m); }

	Point3D operator/(const double d) const { return Point3D(x/d, y/d, z/d); }

	bool operator==(const Point3D &o) const { return fabs(x-o.x) < EPS && fabs(y-o.y) < EPS; }
};

ostream& operator << (ostream& os, const Point3D& p) {
	os << "(" << p.x << ", " << p.y << ", " << p.z << ")";
	return os;
}

double dot(Point3D a, Point3D b) { return a.x * b.x + a.y * b.y + a.z * b.z; }
Point3D cross(Point3D a, Point3D b) { return Point3D(a.y*b.z - a.z*b.y, a.z*b.x - a.x*b.z, a.x*b.y - a.y*b.x); }

double norm(Point3D p) { return dot(p, p); }
double abs(Point3D p) { return sqrt(norm(p)); }

struct Line {
	Point3D a, b;

	Line() : a(Point3D(0, 0, 0)), b(Point3D(0, 0, 0)) {}

	Line(Point3D a, Point3D b) : a(a), b(b) {}
};

ostream& operator << (ostream& os, const Line& l) {
	os << "(" << l.a.x << ", " << l.a.y << ", " << l.a.z <<  ")-(" << l.b.x << "," << l.b.y << ", " << l.b.z <<  ")";
	return os;
}

Point3D project(Line l, Point3D p) {
	Point3D base = l.b - l.a;
	double t = dot(base, p-l.a) / dot(base, base);
	return l.a + base * t;
}

struct Ball {
	Point3D p;
	double r;

	Ball() : p(Point3D(0, 0, 0)), r(0.0) {}

	Ball(Point3D p, double r) : p(p), r(r) {}
};

ostream& operator << (ostream& os, const Ball& b) {
	os << "(" << b.p.z << ", " << b.p.y << ", " << b.p.z << " :" << b.r << ")";
	return os;
}

int main() {
	int n, q;
	cin >> n >> q;

	vector<Ball> v(n);
	vector<ll> cost(n);
	rep(i, n) {
		cin >> v[i].p.x >> v[i].p.y >> v[i].p.z >> v[i].r >> cost[i];
	}

	rep(i, q) {
		ll ans = 0;
		Point3D s, t;
		cin >> s.x >> s.y >> s.z >> t.x >> t.y >> t.z;

		Line line(s, t);

		rep(j, n) {
			Point3D proj = project(line, v[j].p);

			if(abs(line.b - line.a) >= abs(proj - line.a) && abs(line.a - line.b) >= abs(proj - line.b)) {
				if(abs(proj - v[j].p) < v[j].r + EPS) {
					ans += cost[j];
				}
			}
		}

		cout << ans << endl;
	}

	return 0;
}
```

