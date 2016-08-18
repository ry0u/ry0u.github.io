---
layout: post
title: "SRM683 D2M MoveStonesEasy"
date: 2016-03-03 23:54:40 +0900
comments: true
categories: [topcoder, SRM, シュミレーション]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-branding="0"><h4><a href="https://community.topcoder.com/stat?c=problem_statement&pm=14182&rd=16653">TopCoder Statistics - Problem Statement</a></h4><p>There are n piles of stones arranged in a line. The piles are numbered 0 through n-1, in order. In other words, for each valid i, piles i and i+1 are adjacent. You are given two int[]s a and b, each with n elements.</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

現在の石の山と目的の石の山が渡されるので，左から順番に目的の石の山にしていく．

# Code

```cpp
	public:
	int get(vector <int> a, vector <int> b) {
		int n = a.size();

		ll ans = 0;
		ll asum = 0, bsum = 0;

		rep(i, n) asum += a[i];
		rep(i, n) bsum += b[i];

		if(asum != bsum) return -1;

		rep(i, n) {
			if(a[i] == b[i]) continue;
			if(a[i] > b[i]) {
				a[i+1] += a[i] - b[i];
				ans += a[i] - b[i];
			} else {
				int len = 1, j = i+1;
				while(a[i] < b[i]) {
					int d = b[i] - a[i];
					if(a[j] >= d) {
						a[j] -= d;
						a[i] += d;
						ans += d * len;
					} else {
						a[i] += a[j];
						ans += a[j] * len;
						a[j] = 0;
						j++;
						len++;
					}
				}
			}
		}

		return ans;
	}
```
