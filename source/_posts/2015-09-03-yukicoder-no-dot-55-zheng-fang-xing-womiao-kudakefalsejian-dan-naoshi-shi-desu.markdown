---
layout: post
title: "yukicoder No.55 正方形を描くだけの簡単なお仕事です"
date: 2015-09-03 14:28:18 +0900
comments: true
categories: [yukicoder,星2,幾何]
---

http://yukicoder.me/problems/83  

Sample1
{% img /images/yukicoder/055/1.png %}
Sample2 この3点では正方形が出来ない
{% img /images/yukicoder/055/2.png %}
Sample3
{% img /images/yukicoder/055/3.png %}
Sample4
{% img /images/yukicoder/055/4.png %}

# 考察
3点が与えられ，正方形となるもう1点を答えるのだが，正方形がx軸，y軸に平行でなくてもいい．3点の中で最も2点間の距離が長いものを線分として，もう1点をその線分に対して対象な点を求めた．

サンプル3を考える．
{% img /images/yukicoder/055/5.png %}
一番長い線分を見つける．
{% img /images/yukicoder/055/6.png %}
この線分に対し，対象な点を取る．
{% img /images/yukicoder/055/7.png %}
正方形の完成．
{% img /images/yukicoder/055/8.png %}

このままではひし型も作れてしまうので，与えられた3点によって出来る三角形が二等辺三角形かつ，短い辺の{% m %} \sqrt{2} {% em %}が，長い辺となる時のみ行う．

#Code

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

    double cross(const Point &o) const { return x * o.y - y * o.x; }

    double dot(const Point &o) const { return x * o.x + y * o.y; }

    double atan() const { return atan2(y, x); }

    double norm() const { return sqrt(dot(*this)); }

    double distance(const Point &o) const { return (o - (*this)).norm(); }

    double area(const Point &a,const Point &b) {
        Point p = a - (*this), p2 = b - (*this); 
        return p.cross(p2);
    }

    double area_abs(const Point &a,const Point &b) const {
        Point p = a - (*this), p2 = b - (*this);
        return fabs(p.cross(p2)) / 2.0;
    }	

    //線分abが自身に含まれているのかどうか判断する
    int between(const Point &a,const Point &b) {
        if(area(a,b) != 0) return 0;

        if(a.x != b.x)  return ((a.x <= x) && (x <= b.x) || (a.x >= x) && (x >= b.x));
        else return ((a.y <= y) && (y <= b.y) || (a.y >= y) && (y >= b.y));
    }      

    double distance_seg(const Point& a,const Point& b) {
        if((b-a).dot(*this-a) < EPS) {
            return (*this-a).norm();
        }
        if((a-b).dot(*this-b) < EPS) {
            return (*this-b).norm();
        }
        return abs((b-a).cross(*this-a)) / (b-a).norm();
    }

    bool hitPolygon(const Point& a,const Point& b,const Point& c) {
        double t = (b-a).cross(*this-b);
        double t2 = (c-b).cross(*this-c);
        double t3 = (a-c).cross(*this-a);	

        if((t > 0 && t2 > 0 && t3 > 0) || ( t < 0 && t2 < 0 && t3 < 0)) {
            return true;
        }

        return false;
    }
};

struct Seg {
    Point a,b;

    Seg (Point a, Point b) : a(a),b(b) {}

    bool isOrthogonal(Seg &s) { return equals((b - a).dot(s.b - s.a),0.0); }

    bool isParallel(Seg &s) { return equals((b-a).cross(s.b - s.a),0.0); }

    bool isIntersect(Seg &s) {
        if(s.a.between(a,b) || s.b.between(a,b) || a.between(s.a,s.b) || b.between(s.a,s.b)) {
            return true;
        }
        return ((a-b).cross(s.a-a) * (a-b).cross(s.b-a) < EPS) && ((s.b-s.a).cross(a-s.a)*(s.b-s.a).cross(b-s.a) < EPS);
    }

    bool distance(Seg &s) {
        if((*this).isIntersect(s)) return 0.0;

        return min(min(a.distance_seg(s.a,s.b),b.distance_seg(s.a,s.b)),min(s.a.distance_seg(a,b),s.b.distance_seg(a,b)));
    }

    Point getCrossPoint(Seg &s) {
        Point p = s.b - s.a;
        double d = abs(p.cross(a-s.a));
        double d2 = abs(p.cross(b-s.a));

        double t = d / (d+d2);
        return a + (b-a)*t;
    }

    Point project(Point &p) {
        Point base = b - a;
        double t = base.dot(p-a) / base.dot(base);
        return a + base * t;
    }

    Point reflect(Point &p) {
        return p + (project(p) - p) * 2.0;
    }
};

int main() {
    vector<Point> v(3);
    set<int> id;
    rep(i,3) {
        cin >> v[i].x >> v[i].y;
        id.insert(i);
    }

    set<double> st;
    double len = 0, d = INF;
    rep(i,3) {
        int j = (i+1) % 3;
        double dist = v[i].distance(v[j]);
        len = max(len, dist);
        d = min(d, dist);
        st.insert(dist);
    }

    if(st.size() == 2 && equals(2*d*d,len*len)) {
        int s = 0, t = 0;
        rep(i,3) {
            int j = (i+1) % 3;
            if(len == v[i].distance(v[j])) {
                id.erase(i);
                id.erase(j);
                s = i;
                t = j;
                break;
            }
        }

        Seg seg(v[s],v[t]);
        Point p = seg.reflect(v[*(id.begin())]);
        cout << (int)p.x << " " << (int)p.y << endl;

    } else {
        cout << -1 << endl;
    }

    return 0;
}
```
