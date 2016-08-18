---
layout: post
title: "AOJ1086 Live Schedule"
date: 2016-05-12 00:53:50 +0900
comments: true
categories: [AOJ, Vol10, 動的計画法]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-branding="0" data-card-type="article"><h4><a href="http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=1086">Live Schedule | Aizu Online Judge</a></h4><p>YOKARI TAMURAは全国的に有名なアーティストである。今月YOKARIは D 日間にわたってライブツアーを行う。ツアーのスケジュールの決定においてこの国を C 種類の地域でわける。YOKARIがある地域でライブを行うことにより利益を得られ、これは正の整数で表される。YOKARIは原則として 1 日に最大 1 つまでライブを行う。ただし、ある地域でライブを行った後、隣接する地域でライブを行える場合はその地域で同じ日に再びライブを行うことができる。この条件を満たす限り、地域を移動しながら何度もライブを行うことができる。また、同じ日に同じ地域でライブを 2 度以上行うことはできない。さらに、同じ日に 2 回以上のライブを行う日の数はツアー期間中合計 X 以下でなければならない。</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

{% math %}
	dp[i][j][k] := i日目に疲労度jで2回以上ライブを行った回数がkの時の売上の最大値
{% endmath %}

として，動的計画法．ライブをし終わった地域から，次の日は開始かと思っていて状態に"町{% m %} i {% em %}"にいる時，というのを追加していたがいらなかった．ライブを始める町と終わる町はどこでも良いので {% m %} C ^2 {% em %}で区間を全探索し， その区間の疲労度を足した次の日に遷移可能とした．{% m %} 2 {% em %}回以上続けてライブを続けない場合は制限がないので，別に分けた．  
WAが取れなかった原因は，ライブを行わない，というのを追加していないことだった．区間は同じだが後々にライブをした方が利益が高い場合があるので，この行動を追加しなければおかしかった．  
また {% m %} E_{i, j} = 0 {% em %}の時にはライブを行うことが出来ない，という条件に気が付かず時間を溶かした．

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
#include <iomanip>

#define REP(i,k,n) for(int i=k;i<n;i++)
#define rep(i,n) for(int i=0;i<n;i++)
#define INF 1<<30
#define pb push_back
#define mp make_pair

using namespace std;
typedef long long ll;
typedef pair<int,int> P;

// i日目に疲労度jで2回以上のライブ回数kの時の売上の最大値
ll dp[35][55][10];
ll E[20][35], F[20][35];

int main() {
	ll c, d, w, x;
	while(cin >> c >> d >> w >> x) {
		if(c == 0 && d == 0 && w == 0 && x == 0) break;

		memset(E, 0, sizeof(E));
		rep(i, c) {
			rep(j, d) cin >> E[i][j];
		}

		memset(F, 0, sizeof(F));
		rep(i, c) {
			rep(j, d) cin >> F[i][j];
		}

		// rep(i, c) {
		// 	rep(j, d) {
		// 		cout << "(" << E[i][j] << ", " << F[i][j] << ") ";
		// 	}
		// 	cout << endl;
		// }

		memset(dp, 0, sizeof(dp));

		rep(i, d) {
			rep(j, w + 1) {
				rep(k, x + 1) {
					dp[i+1][j][k] = max(dp[i+1][j][k], dp[i][j][k]);

					rep(s, c) {
						if(E[s][i] == 0) continue;

						ll sum = E[s][i];
						ll res = F[s][i];

						if(j + res <= w) {
							dp[i+1][j+res][k] = max(dp[i+1][j+res][k], dp[i][j][k] + sum);
						}

						REP(t, s + 1, c) {
							if(E[t][i] == 0) break;

							sum += E[t][i];
							res += F[t][i];

							if(j + res <= w && k + 1 <= x) {
								dp[i+1][j+res][k+1] = max(dp[i+1][j+res][k+1], dp[i][j][k] + sum);
							}
						}
					}
				}
			}
		}

		// rep(i, d + 1) {
		// 	cout << " ----- day : " << i << endl;
		// 	rep(j, w + 1) {
		// 		rep(k, x + 1) {
		// 			cout << setw(3) << dp[i][j][k] << " ";
		// 		}
		// 		cout << endl;
		// 	}
		// }
		
		ll ans = 0;
		// rep(i, d + 1) {
			rep(j, w + 1) {
				rep(k, x + 1) {
					ans = max(ans, dp[d][j][k]);
				}
			}
		// }

		cout << ans << endl;
	}

	return 0;
}
```
