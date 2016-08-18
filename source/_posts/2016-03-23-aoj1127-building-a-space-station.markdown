---
layout: post
title: "AOJ1127 Building a Space Station"
date: 2016-03-23 20:27:09 +0900
comments: true
categories: 
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-type="article" data-card-branding="0"><h4><a href="http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=1127">Building a Space Station</a></h4><p>You are a member of the space station engineering team, and are assigned a task in the construction process of the station. You are expected to write a computer program to complete the task. The space station is made up with a number of units, called cells.</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

最小全域木を作る．小数点{% m %} 20 {% em %}桁ぐらい出力していて， {% m %} 3 {% em %}桁固定というのを見逃していた．

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

struct Sphere {
	Point3D p;
	double r;

	Sphere() : p(Point3D(0, 0, 0)), r(0.0) {}

	Sphere(Point3D p, double r) : p(p), r(r) {}
};

ostream& operator << (ostream& os, const Sphere& s) {
	os << "(" << s.p.z << ", " << s.p.y << ", " << s.p.z << " :" << s.r << ")";
	return os;
}

double distanceSphSph(Sphere s1, Sphere s2) {
	Point3D p = s2.p - s1.p;
	double dist = abs(p) - s1.r - s2.r;
	return max(0.0, dist);
}

struct UnionFind {
	vector<int> par,rank;
	int N;

	UnionFind(int n) {
		N = n;
		par.resize(n);
		rank.resize(n);

		rep(i,n) {
			par[i] = i;
			rank[i] = 0;
		}
	}

	int find(int x) {
		if(par[x] == x) return x;
		else return par[x] = find(par[x]);
	}

	void unite(int x,int y) {
		x = find(x);
		y = find(y);

		if(x == y) return;

		if(rank[x] < rank[y]) {
			par[x] = y;
		}
		else {
			par[y] = x;
			if(rank[x] == rank[y]) rank[x]++;
		}
	}

	bool same(int x,int y) {
		return find(x) == find(y);
	}

	int size() {
		int cnt = 0;
		rep(i,N) if(find(i) == i) cnt++;
		return cnt;
	}
};

struct edge {
	int from,to;
	double cost;

	edge(int t, int c) : to(t),cost(c) {}
	edge(int f, int t, double c) : from(f),to(t),cost(c) {}

	bool operator<(const edge &e) const {
		return cost < e.cost;
	}
};

double kruskal(int n, vector<edge> v) {
	sort(v.begin(),v.end());
	UnionFind uf(n);

	double ret = 0;
	rep(i, v.size()) {
		edge e = v[i];
		if(!uf.same(e.from,e.to)) {
			uf.unite(e.from,e.to);
			ret += e.cost;
		}
	}
	return ret;
}

int main() {
	int n;

	while(cin >> n && n) {
		vector<Sphere> s(n);
		rep(i, n) cin >> s[i].p.x >> s[i].p.y >> s[i].p.z >> s[i].r;

		vector<edge> v;
		rep(i, n) {
			REP(j, i+1, n) {
				double dist = distanceSphSph(s[i], s[j]);
				v.push_back(edge(i, j, dist));
			}
		}

		cout << fixed;
		cout.precision(3);
		cout << kruskal(n, v) << endl;
	}

	return 0;
}
```

