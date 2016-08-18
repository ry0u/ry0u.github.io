---
layout: post
title: "SRM682 D2H FriendlyRobot"
date: 2016-02-29 01:13:10 +0900
comments: true
categories: [topcoder, SRM, 動的計画法]
---

文字列{% m %} U,D,R,L {% em %}の命令で動くロボットがある．この文字列を{% m %} K {% em %}回書き換えられることが出来るときに，最大で何回{% m %}(0,0){% em %}を通ることが出来るか．

<!-- more -->

---

移動回数が奇数回の時に{% m %} (0,0) {% em %}に戻ることは出来ない．また偶数回の場合でも横方向，縦方向の移動量が共に偶数回，共に奇数回の場合のみ戻ることが出来る．  
また，{% m %} +を-に，-を+{% em %}に変えることで{% m %} 2 {% em %}移動量が変わる．  
そして，戻るために命令を書き換えなければならない回数も以下のように一意に求まる．
{% math %}
\begin{eqnarray}
	共に偶数回 &:=&  \frac{横方向}{2} + \frac{縦方向}{2} \\
	共に奇数回 &:=&  \frac{横方向}{2} + \frac{縦方向}{2} + 1\\
\end{eqnarray}
{% endmath %}
共に奇数回の場合に{% m %} +1 {% em %}されるのは，例えば命令が{% m %} UR {% em %}の場合に{% m %} UD {% em %}に書き換えることで{% m %} (0, 0) {% em %}に戻れるためである．

これより
{% math %}
	dp[i][j] := i番目までの命令列をj回変更した時の(0, 0)を訪れる最大値
{% endmath %}
として，総当りしてmaxを取った．

# Code

```cpp
	public:
	int findMaximumReturns(string s, int K) {

		vector<int> X(s.size() + 1), Y(s.size() + 1);
		int x = 0, y = 0;
		rep(i, s.size()) {
			if(s[i] == 'U') y++;
			if(s[i] == 'D') y--;
			if(s[i] == 'R') x++;
			if(s[i] == 'L') x--;

			X[i+1] = x; Y[i+1] = y;
		}

		rep(i, s.size() + 1) {
			rep(j, K + 1) {
				dp[i][j] = -1;
			}
		}
		dp[0][0] = 0;

		rep(i, s.size() + 1) {
			rep(j, K + 1) {
				if(dp[i][j] == -1) continue;
				for(int k = i + 2; k < s.size() + 1; k += 2) {
					int s = abs(X[k] - X[i]), t = abs(Y[k] - Y[i]);
					int res = 0;

					if(s % 2 == 0 && t % 2 == 1) continue;
					if(s % 2 == 1 && t % 2 == 0) continue;
					if(s % 2 == 0 && t % 2 == 0) res = s / 2 + t / 2;
					if(s % 2 == 1 && t % 2 == 1) res = s / 2 + t / 2 + 1;

					if(j + res > K) continue;
					dp[k][j + res] = max(dp[k][j + res], dp[i][j] + 1);
				}
			}
		}

		ll ans = 0;
		rep(i, s.size() + 1) {
			rep(j, K + 1) {
				ans = max(ans, dp[i][j]);
			}
		}

		return ans;
	}
```
