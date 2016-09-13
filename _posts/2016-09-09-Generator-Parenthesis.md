---
layout:     post
title:      "22.Generate Parentheses-生成括号组"
subtitle:   " \"LeetCode\""
date:       2016-9-9 16:05:03 
author:     "Zering"
header-img: "img/road.jpg"
catalog: true
tags:
    - LeetCode
---
### 问题
问题地址：[https://leetcode.com/problems/generate-parentheses/](https://leetcode.com/problems/generate-parentheses/)

Given n pairs of parentheses, write a function to generate all combinations of well-formed parentheses.

For example, given n = 3, a solution set is:

	[
	  "((()))",
	  "(()())",
	  "(())()",
	  "()(())",
	  "()()()"
	]

### 问题翻译
给你n组括号，写一个能生成所有合法的括号组合的功能。

示例，当n = 3,得到的组合是：

	[
	  "((()))",
	  "(()())",
	  "(())()",
	  "()(())",
	  "()()()"
	]

### 问题分析
这个问题是一个生成所有组合的策略，是一个回溯（backtracking）的问题，可以使用递归的方式，产生所有的组合，然后选出其中符合条件的组合；

此类问题的思考方式：**在当前局面下，你有若干种选择。那么尝试每一种选择。如果已经发现某种选择肯定不行（因为违反了某些限定条件），就返回；如果某种选择试到最后发现是正确解，就将其加入解集**

思考递归题时，只要明确三点就行：**选择 (Options)，限制 (Restraints)，结束条件 (Termination)**

在本题中

**选择：**
1. 加左括号；
2. 加右括号；

**限制：**
1. 左括号不少于右括号；
2. 左括号用完了；（有1.的限制所以当右括号也用完时，即为结束）；

**结束条件：**
左右括号都用完了

伪代码如下：
	
	if (左右括号都已用完) {
	  加入解集，返回
	}
	if (左括号剩余数多余右括号剩余){
		返回	
	}
	//否则开始试各种选择
	if (还有左括号可以用) {
	  加一个左括号，继续递归
	}
	if (还有右括号可以用) {
	  加一个右括号，继续递归
	}

### 问题解答（java示例）
	public static void backtrack(String sublist, List<String> res, int left, int right) {
        if (left == 0 && right == 0) {
            res.add(sublist);
            return;
        }
        if (left > right)
            return;
        if (left > 0)
            backtrack(sublist + "(", res, left - 1, right);
        if (right > 0)
            backtrack(sublist + ")", res, left, right - 1);
    }

### 后记
练习源码地址：[https://github.com/Zering/LeetCode](https://github.com/Zering/LeetCode)