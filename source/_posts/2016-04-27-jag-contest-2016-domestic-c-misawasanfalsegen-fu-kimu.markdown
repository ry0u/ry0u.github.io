---
layout: post
title: "JAG Contest 2016 Domestic C みさわさんの根付き木"
date: 2016-04-27 23:21:58 +0900
comments: true
categories: [模擬予選, 木]
---

<blockquote class="embedly-card" data-card-key="39deea93f79745829254c0652225a544" data-card-controls="0" data-card-branding="0" data-card-type="article"><h4><a href="http://jag2016-domestic.contest.atcoder.jp/tasks/jag2016secretspring_c">C: みさわさんの根付き木 - JAG Contest 2016 Domestic | AtCoder</a></h4><p>(null)</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<!-- more -->

最初に，完全2分木にして配列を用いて表せば実装簡単だと思い，書き始めたが深さが大きい時に対応出来ないことに気づき，配列ではなくmapにした．一応書き終わり，サンプルを試している内にそもそもこの方法ではノード番号がlong longで収まり切らないことに気づいた．  
次に純粋に文字列を木に直して，rootからmergeしていく方法にしてACが取れた．正しい方針が立てれずに時間を無駄にしてしまったので反省したい．


# Code

```cpp
#include <iostream>
#include <sstream>
#include <string>
#include <cstring>
#include <vector>
#include <algorithm>
#include <map>

#define REP(i, k, n) for(int i = k; i < n; i++) 
#define rep(i, n) for(int i = 0; i < n; i++) 

using namespace std;

struct Tree {
	int v;
	Tree* left;
	Tree* right;

	Tree(int v) : v(v) {}
};

void f(string s, Tree* node) {
	string t = "";
	REP(i, 1, s.size()-1) {
		t += s[i];
	}

	if(t == "") return;
	s = t;

	int root = 0, sum = 0;
	string left = "";

	int i = 0;
	for(i = 0; i < s.size(); i++) {
		left += s[i];

		if(s[i] == '(') sum++;
		else if(s[i] == ')') sum--;

		if(sum == 0){
			stringstream ss;
			i += 2;
			REP(j, i, s.size()) {
				if(s[j] == ']') {
					i++;
					break;
				} else {
					ss << s[j];
					i++;
				}
			}
			ss >> root;
			node->v = root;
			break;
		}
	}

	string right = "";
	for(; i < s.size(); i++) {
		right += s[i];
	}

	node->left = new Tree(-1);
	f(left, node->left);

	node->right = new Tree(-1);
	f(right, node->right);
}

void merge(Tree* res, Tree* node, Tree* node2) {
	res->v = node->v + node2->v;
	// cout << "-------- merge:" << res->v << " " << node->v << " " << node2->v << endl;

	res->left = new Tree(-1);
	if(node->left != NULL && node->left->v != -1 && node2->left != NULL && node2->left->v != -1) {
		merge(res->left, node->left, node2->left);
	}

	res->right = new Tree(-1);
	if(node->right != NULL && node->right->v != -1 && node2->right != NULL && node2->right->v != -1) {
		merge(res->right, node->right, node2->right);
	}
}

string dfs(Tree* res) {
	if(res->v == -1) {
		return "()";
	}

	stringstream ss;
	ss << res->v;
	return "(" + dfs(res->left) + "[" + ss.str() + "]" + dfs(res->right) + ")";
}

int main() {
	string s, t;
	cin >> s >> t;

	s = "(" + s + ")";
	t = "(" + t + ")";

	Tree *root = new Tree(-1);
	f(s, root);
	
	Tree *root2 = new Tree(-1);
	f(t, root2);
	
	Tree *res = new Tree(-1);
	merge(res, root, root2);
	
	string ret = dfs(res);

	// 最後の()を取る.
	REP(i, 1, ret.size()-1) {
		cout << ret[i];
	}
	cout << endl;


	return 0;
}

```


