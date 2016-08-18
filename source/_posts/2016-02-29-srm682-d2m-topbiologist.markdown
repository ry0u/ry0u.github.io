---
layout: post
title: "SRM682 D2M TopBiologist"
date: 2016-02-29 00:36:01 +0900
comments: true
categories: [topcoder, SRM, シュミレーション]
---

大文字アルファベットの{% m %}A, C, G, T{% em %}で構成される文字列が与えられる．この文字列に含まれない最小の{% m %}A, C, G, T{% em %}で構成される文字列を返す．

<!-- more -->

---

与えられる文字列の長さは最大で{% m %}2000{% em %}なので長さ{% m %}6{% em %}の文字列が最大となる（長さ{% m %}5{% em %}の文字列を単純に連結すれば{% m %}5120{% em %}になるが上手いこと組み合わせれば{% m %}2000{% em %}以下になる）．  

愚直に探索し，その文字列が見つからなければ返す．  
部分文字列を全て列挙しmapに突っ込んだらMLEして落とした．

# Code
```cpp
	public:
	string findShortestNewSequence(string s) {
		m.clear();

		rep(i, s.size()) {
			stringstream ss;
			REP(j, i, s.size()) {
				string t = s.substr(i, j-i+1);
				if(t.size() >= 10) continue;
				m[t] = true;
			}
		}

		que.push("A");
		que.push("C");
		que.push("G");
		que.push("T");

		string ans = "";
		while(que.size()) {
			string t = que.front();
			que.pop();

			if(!m[t]) {
				ans = t;
				break;
			}

			que.push(t + "A");
			que.push(t + "C");
			que.push(t + "G");
			que.push(t + "T");
		}

		// last check
		return ans;
	}
```
