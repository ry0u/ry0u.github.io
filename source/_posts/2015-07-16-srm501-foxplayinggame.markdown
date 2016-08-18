---
layout: post
title: "SRM501 FoxPlayingGame"
date: 2015-07-16 02:56:16 +0900
comments: true
categories: [topcoder,srm]
---

scoreAをnA回足す，scoreBをnB回掛ける．この操作による最大値を求める．また

{% math %}
scoreA = paramA / 1000.0 \\
scoreB = paramB / 1000.0
{% endmath %}

として与えられる．


# 考察

掛ける場合は最初の0の状態に掛けることができるので，回数を調整できるが，足す場合はそのようなことができない．よって，まずscoreAをnA回足す．また，この値が+か-かによって場合分けをする  

---
- +の場合かつscoreBが1以上の場合は，全て掛ける．  
- +の場合かつscoreBが-1以下の場合は，(scoreB)^2は正なので偶数回掛けるのが最大  
- +の場合かつscoreBがそれ以外の場合は，減少するため，最初の0に掛けて消す

---
- -の場合かつscoreBが1以上の場合は，負の値が増加してしまうので掛けない
- -の場合かつscoreBが-1以下の場合は，奇数回掛けることによって値を正にすることができる
- -の場合かつscoreBが0より大きく1以下の場合は，負の値を小さくすることができるので全部掛ける
- -の場合かつscoreBが-1以上0より小さい場合は，一度だけ掛けて値を正することが最大


# Code

```cpp
#include <iostream>
#include <vector>
#include <string>
#include <sstream>
#include <algorithm>
#include <set>
#include <map>

#define REP(i,k,n) for(int i=k;i<n;i++)
#define rep(i,n) for(int i=0;i<n;i++)

using namespace std;
typedef long long ll;

class FoxPlayingGame {
	public:
	double theMax(int nA, int nB, int paramA, int paramB) {
        double A = paramA / 1000.0;
        double B = paramB / 1000.0;

        double res = nA * A;
        if(res > 0) {
            if(B <= -1) {
                nB -= nB%2;
                rep(i,nB) {
                    res *= B;
                }
            }else if(B >= 1) {
                rep(i,nB) {
                    res *= B;
                }
            }
        }else {
            if(0 <= B && B < 1.0) {
                rep(i,nB) {
                    res *= B;
                }
            }
            else if(-1.0 < B && B < 0) {
                if(nB != 0) res *= B;
            }
            else if(B <= -1) {
                if(nB%2 == 0) nB--;
                rep(i,nB) {
                    res *= B;
                }
            }
        }

        return res;

	}
};

```

SystemTestに落ちまくり．特にscoreAが負，scoreBが負の時に，一度だけscoreBを掛ければよかったが，ここでnBが0の時の処理を加えればいいことに全く気づけなかった．
