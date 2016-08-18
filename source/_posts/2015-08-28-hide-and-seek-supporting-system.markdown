---
layout: post
title: "Hide-and-Seek Supporting System"
date: 2015-08-28 02:23:10 +0900
comments: true
categories: [AOJ,幾何]
---

http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=0129  

サンプル1  
{% img /images/AOJ/0129.png %}

円に接してる場合は，Safe判定となる．

# 考察

円の中心と，線分の関係を見る．Dangerの場合は線分の端点両方が円内包されていない，かつ円の中心と線分の距離が円の半径よりも小さい時である．前回，この問題を取り組んだ時はWAを連発したが，今回はACすることが出来た．

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

struct Circle {
    Point p;
    double r;

    Circle() : p(Point(0,0)), r(0) {}

    Circle(Point o, double r) : p(o), r(r) {}

    Circle(double x,double y, double r) : p(Point(x,y)), r(r) {}

    bool isCircleIn(const Point& o) {
        Point res = o-p;
        return res.dot(res) < r*r + EPS;
    }

    // 1:外で接する，0:交差なし，-1:内で接する，2:交差，-2:内包
    int isIntersect(const Circle& c) {
        double d = (c.p - p).dot(c.p - p);
        double len = (c.r + r) * (c.r + r);

        if(equals(d,len)) return 1;
        if(d > len) return 0;

        double R = fabs(c.r - r) * fabs(c.r - r);
        if(equals(d,R)) return -1;
        if(d > R) return 2;
        return -2;
    }

    vector<Point> getCrossPoint(const Circle& c) {
        vector<Point> ret;
        int ch = isIntersect(c);

        if(ch == 0 || ch == -2) return ret;

        Point base = c.p - p;
        double len = base.dot(base);
        double t = (r*r - c.r*c.r + len) / (2.0 * len);

        if(ch == 2) {
            Point n(-base.y,base.x);
            n = n / (n.norm());
            double h = sqrt(r * r - t*t*len);

            ret.push_back(p + (base*t) + (n*h));
            ret.push_back(p + (base*t) - (n*h));
        } else {
            ret.push_back(p + (base*t));
        }

        return ret;
    }
};

int main() {
    int n,m;

    while(cin >> n && n) {
        vector<Circle> v;
        rep(i,n) {
            double x,y,r;
            cin >> x >> y >> r;

            v.push_back(Circle(x,y,r));
        }

        int m;
        cin >> m;

        rep(q,m) {
            Point t,o;
            cin >> t.x >> t.y >> o.x >> o.y;

            bool flag = true;

            rep(i,n) {
                if(v[i].isCircleIn(t) && v[i].isCircleIn(o)) {
                    continue;
                } else if(!v[i].isCircleIn(t) && !v[i].isCircleIn(o)) {
                    double d = v[i].p.distance_seg(t,o);
                    if(d < v[i].r + EPS) flag = false;
                } else {
                    flag = false;
                }
            }

            if(flag) cout << "Danger" << endl;
            else cout << "Safe" << endl;
        }
    }

    return 0;
}
```

