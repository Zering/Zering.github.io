---
layout:     post
title:      "5. Longest Palindromic Substring-最长回文子字符串"
subtitle:   " \"LeetCode\""
date:       2016-9-23 17:11:30 
author:     "Zering"
header-img: "img/road.jpg"
catalog: true
tags:
    - LeetCode
---

## 最长回文子字符串

### 问题

问题地址：[https://leetcode.com/problems/longest-palindromic-substring/](https://leetcode.com/problems/longest-palindromic-substring/)

Given a string S, find the longest palindromic substring in S. You may assume that the maximum length of S is 1000, and there exists one unique longest palindromic substring.

### 问题翻译

给一个字符串S，找出S中最长的回文字符串。假设S的最大长度为1000,且只有一个最长的回文子字符串。

### 问题分析

简单思路：找到所有的回文子字符串，返回长度最长的一个

#### 解法一，暴力检索（O(n3)）

遍历出所有的子字符串,判断是否为回文，返回最长的回文

java代码示例：

	public String longestPalindrome(String s) {
		if(s.isEmpty()||s == null)
			return null;
		int beginIndex = 0,endIndex = 0,longestLength = 0;
		String subString;
		for(int i=0;i<s.length();i++){
			for(int j = i + 1;j < s.length();j++){
				subString = s.substring(i,j);
				if(isPalindrome(subString) && subString.length() > longestLength){
					longestLength = subString.length();
					beginIndex = i;
					endIndex = j;
				}
			}
		}
		return s.substring(beginIndex, endIndex);
	}
	
	private boolean isPalindrome(String string) {
		if (string == null || string.isEmpty()) {
			return false;
		}
		for(int i=0;i < string.length()/2; i++){
			if(string.charAt(i) != string.charAt(string.length()-i-1))
				return false;
		}
		return true;
	}

注：本方法时间复杂度为O(n3)，耗时太长，不被ac

### 解法二，中心扩展（O（n2））

遍历字符串的每一个字符，判断以该字符为中心的最长的回文

java代码示例

	public String longestPalindromeExpandAroundCenter(String s) {
		if(s == null || s.isEmpty())
			return null;
		int beginIndex=0;
		int endIndex=0;
		for(int i = 0;i < s.length();i++){
			int len1 = expandAroundCenter(s, i, i); 
			int len2 = expandAroundCenter(s, i, i+1);
			int len = Math.max(len1, len2);
			if(len > endIndex-beginIndex){
				beginIndex = i - (len-1)/2;
				endIndex = i + len/2;
			}
		}
		return s.substring(beginIndex,endIndex+1);
	}
	
	private int expandAroundCenter(String s,int left,int right){
		int L = left;
		int R = right;
		while(L >= 0 && R < s.length() && s.charAt(L) == s.charAt(R)){
			L--;
			R++;
		}
		return R-L-1;
	}

### 后记

源码：[https://github.com/Zering/LeetCode](https://github.com/Zering/LeetCode)

网上有一种O(n)的解法：[http://articles.leetcode.com/longest-palindromic-substring-part-ii](http://articles.leetcode.com/longest-palindromic-substring-part-ii)


