---
layout:     post
title:      "74. Search a 2D Matrix & 240.Search a 2D Matrix II-搜索二维矩阵"
subtitle:   " \"LeetCode\""
date:       2016-11-22 14:26:17 
author:     "Zering"
header-img: "img/road.jpg"
catalog: true
tags:
    - LeetCode
---

## 搜索二维矩阵

### 问题

问题地址：

[74. Search a 2D Matrix](https://leetcode.com/problems/search-a-2d-matrix/)

[240.Search a 2D Matrix II](https://leetcode.com/problems/search-a-2d-matrix-ii/)

Write an efficient algorithm that searches for a value in an m x n matrix. This matrix has the following properties:

1. Integers in each row are sorted in ascending from left to right.
2. Integers in each column are sorted in ascending from top to bottom.

For example,

Consider the following matrix:

	[
	  [1,   4,  7, 11, 15],
	  [2,   5,  8, 12, 19],
	  [3,   6,  9, 16, 22],
	  [10, 13, 14, 17, 24],
	  [18, 21, 23, 26, 30]
	]
Given target = 5, return true.

Given target = 20, return false.

### 问题翻译

写一个高效的算法来搜索一个m*n的矩阵中的值，这个矩阵有以下特点：

1. 每一行都是从左到右升序排列的，
2. 每一列也都是从头到底升序排列的。

例如，下面这个矩阵：

	[
	  [1,   4,  7, 11, 15],
	  [2,   5,  8, 12, 19],
	  [3,   6,  9, 16, 22],
	  [10, 13, 14, 17, 24],
	  [18, 21, 23, 26, 30]
	]

给一个target = 5 ，返回ture。

给一个target = 20 ，返回false。

### 问题分析

根据矩阵的规律，我们可以看出右上角的数字大于本行的所有数字，小于本列的所有数字，当我们以该数字为起点时，如果该数组等于要查找的数字，直接返回；如果该数字大于要查找的数字，本列所有数字都大于要查找的数字，所以剔除这个数组所在的列；如果该数字小于要查找的数字，本行所有数字都小于要查找的数字，剔除这个数组所在的行。

### Java解决方案示例

	public class Searcha2DMatrix {
		public boolean searchMatrix(int[][] matrix, int target) {
			if (matrix.length == 0) return false;
	        boolean found = false;
	        int rows = matrix.length;
	        int columns = matrix[0].length;
	        int row = 0;
	        int col = columns-1;
	        while (row < rows && col >= 0) {
				if (matrix[row][col] == target) {
					found = true;
					break;
				} else if (matrix[row][col] > target) {
					-- col;
				} else {
					++ row;
				}
			}
	        return found;
	    }
	}

### 后记

源码：[https://github.com/Zering/LeetCode](https://github.com/Zering/LeetCode)


