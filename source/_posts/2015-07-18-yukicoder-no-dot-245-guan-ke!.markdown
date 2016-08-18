---
layout: post
title: "yukicoder No.245 貫け！"
date: 2015-07-18 02:06:23 +0900
comments: true
categories: [yukicoder, 幾何]
---

http://yukicoder.me/problems/507  

# 考察
端点のペアを列挙し，その直線とN個の線分が交差しているかを見る．直線ではなく，線分で交差判定をしていたためにWA．

以下の場合を考える．
{% img /images/yukicoder245/image1.png %}  
今見ている．端点のペアは緑の点(a,b)だとする．ここで線分を作り交差判定をすると上の線分がcountされない．よって線分ではなく直線にしたい．

まず(a-b)のベクトルを作る．  
{% img /images/yukicoder245/image2.png %}  
aに足す  
{% img /images/yukicoder245/image3.png %}  
(b-a)のベクトルを作る．  
{% img /images/yukicoder245/image4.png %}  
bに足す  
{% img /images/yukicoder245/image5.png %}  
直線とみなせる．  
{% img /images/yukicoder245/image6.png %}  
今回のInputは-100~100までなので，このベクトルを500倍をとって直線とみなした．

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
#include <queue>

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

    bool operator<(const Seg &o) const { 
        return a == o.a ? a < o.a : b < o.b;
    }

    void print() {
        cout << "(" << a.x << "," << a.y << ") (" << b.x << "," << b.y << ")" << endl;
    }
};

int main() {
    int n;
    cin >> n;

    vector<Seg> segs;
    vector<Point> v;

    rep(i,n) {
        double a,b,c,d;
        cin >> a >> b >> c >> d;

        Point p1(a,b);
        Point p2(c,d);

        v.push_back(p1);
        v.push_back(p2);

        segs.push_back(Seg(p1,p2));
    }

    int ans = 0;
    rep(i,v.size()) {
        rep(j,v.size()) {
            if(i == j) continue;
            
            Point a = (v[i] - v[j]) * 500;
            Point b = (v[j] - v[i]) * 500;

            Seg seg(v[i]+a,v[j]+b);

            int cnt = 0;
            rep(k,segs.size()) {
                if(seg.isIntersect(segs[k])) {
                    cnt++;
                }
            }

            ans = max(ans,cnt);
        }
    }

    cout << ans << endl;

    return 0;
}
```
