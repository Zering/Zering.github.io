---
layout:     post
title:      "115. Distinct Subsequences-不同的子序列"
subtitle:   " \"LeetCode\""
date:       2017-2-22 20:25:47  
author:     "Zering"
header-img: "img/road.jpg"
catalog: true
tags:
    - [LeetCode,Java]
---

## 不同的子序列数

### 问题

问题地址：

[115. Distinct Subsequences](https://leetcode.com/problems/distinct-subsequences/?tab=Description)

Given a string S and a string T, count the number of distinct subsequences of T in S.

A subsequence of a string is a new string which is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (ie, "ACE" is a subsequence of "ABCDE" while "AEC" is not).

Here is an example:

	S = "rabbbit", T = "rabbit"

Return `3`.

### 问题翻译

给一个字符串S和一个字符串T，计算出T在S中有多少个不同的子序列。

子序列：通过删除原字符串中的一些（不能一个都不删）字符且不打乱剩下字符的顺序所得到的字符串。（如："ACE"是"ABCDE"的一个子序列，但"AEC"就不是）

示例：

	S = "rabbbit", T = "rabbit"

返回 3

### 问题分析

这里提供网上的一种思路：

** 创建一个二维数组mem[i+1][j+1],其中S[0..j]表示其在T[0..i]中出现的不同的子序列次数，最终mem[T.length()][S.length()]就是需要的结果。**
 
逐行建立这个二元数组：

1. 第一行都填充为1，因为空字符串是空字符串的子序列但只能算作1次。即对每一个j有mem[0][j] = 1
2. 第一列填充为0，因为空字符串不能包括任何的非空字符串（但mem[0][0] = 1）,因此我们得到以下结构：

	  S 0123....j
	T +----------+
	0 |1111111111|
	1 |0         |
	2 |0         |
	3 |0         |
	. |0         |
	. |0         |
	i |0         |

3. 接下来我们要填充整个表格：对每一个（x,y），检查S[x] == T[y],如果为真，则当前项为前一项和上一行的前一项的和；否则，该项与前一项相同。理由如下：
	1. 如果S的当前字符不等于T的当前字符，由于没有新的字符，所以它与前一项相同；
	2. 如果S的当前字符等于T的当前字符，则当前子序列的次数的次数就应该是：前一行包含当前字符之前的T出现在包含当前字符之前的S中的次数**加上**本行前一项的次数。

示例：

S: [acdabefbc]  T: [ab]

第一行，记录a在S中出现的次数，即mem[1]

	           *  *
	      S = [acdabefbc]
	mem[1] = [0111222222]

然后第二行记录ab在s中出现的次数，即mem[2]

               *  * ]
      S = [acdabefbc]
mem[1] = [0111222222]
mem[2] = [0000022244]

所以最后返回的结果为 4，所有的子序列如下：

      S = [a   b    ]
      S = [a      b ]
      S = [   ab    ]
      S = [   a   b ]

### Java解决方案示例

	public class Solution {
	    public int numDistinct(String s, String t) {
	        int[][] mem = new int[t.length()+1][s.length()+1];
	        
	        for(int j = 0; j<=s.length(); j++){
	            mem[0][j] = 1;
	        }
	        
	        for(int i=0; i < t.length(); i++){
	            for(int j=0; j < s.length();j++){
	                if(t.charAt(i) == s.charAt(j)){
	                    mem[i+1][j+1] = mem[i][j] + mem[i+1][j];                    
	                } else {
	                    mem[i+1][j+1] = mem[i+1][j];
	                }
	            }
	        }
	        return mem[t.length()][s.length()];    
	    }
	}

### 后记

源码：[https://github.com/Zering/LeetCode](https://github.com/Zering/LeetCode/blob/master/src/main/java/com/algorithm/DistinctSubsequences.java)


