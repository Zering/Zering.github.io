---
layout:     post
title:      "任意连续子向量中的最大和"
subtitle:   " \"编程珠玑\""
date:       2017-2-14 10:16:04  
author:     "Zering"
header-img: "img/post-bg-java.jpg"
catalog: true
tags:
    - Tips
---

## 任意连续子向量中的最大和

### 前言

本篇来自编程珠玑第八章的一个问题

---

### 正文

### 问题描述

问题的输入时具有n个浮点数的向量x，输出是输入向量的任意连续子向量中的最大和。例如输入向量包含以下10个元素

	31 -41 59 26 -53 58 97 -93 -23 84
		   ↑            ↑
		   2            6

那么该程序输出为x[2..6]的和，即187.

### 问题解答

演进流程：浅显程序--》两个平方算法--》分治算法--》扫描算法

#### 浅显程序

思路：对所有满足0≤i≤j＜n的（i,j）整数对进行迭代，对每个整数对，都要计算x[i..j]的总和，并验证该总和是否大于迄今为止的最大总和。

算法1伪代码：
	
	maxsofar = 0
	for i = [0,n)
		for j = [0,n)
			sum = 0
			for k = [i,j]
	 			sum += x[k]
			maxsofar = max(maxsofar,sum) 

该程序的步数数量级达到O(n3)。


#### 两个平方算法

第一个平方算法：x[i..j]的总和与前面已经计算出的总和（x[i..j-1]的总和）密切相关，因此得出算法2的伪代码：

	maxsofar = 0
	for i = [0,n)
		sum = 0
		for j = [i,n)
			sum += x[j]
			maxsofar = max(maxsofar,sum)

第二个平方算法：通过访问在外循环执行之前就已构建的数据结构的方式在内循环中计算总和。cumarr中的第i个元素包含x[i..j]中各个数的总和可以通过计算cumarr[j]-cumarr[i-1]得到。从而得到算法2b的代码：

	cumarr[-1] = 0
	for i = [0,n)
		cumarr[i] = cumarr[i-1] + x[i]
	maxsofar = 0
	for i = [0,n)
		for j = [i,n)
			sum = cumarr[j] - cumarr[i-1]
			maxsofar = max(maxsofar,sum)


#### 分治算法

分治原理：**要解决规模为n的问题，可递归地解决两个规模近似为n/2的子问题，然后对它们的答案进行合并以得到整个问题的答案**

在本问题中，我们将向量分为左右两个大小近似相等的子向量，分别为a,b向量；

那么最大的子向量，要么整个在a中称为ma，要么整个在b中称为mb,要么跨越a,b的边界称为mc；

用递归的方式我们可以轻易的算出ma和mb，而mc则为a中包含右边界的最大子向量与b中包含左边界的最大子向量，因此得到算法3：
	
	float maxsum3(1,u)
		if(1 > u)
			return 0
		if(1 = u)
			return max(0,x[1])
		m = (1 + u)/2
		lmax = sum = 0
		for(i = m; i >= 1; i++)
			sum += x[i]
			lmax = max(lmax,sum)
		rmax = sum = 0
		for i = (m,u)
			sum += x[i]
			rmax = max(rmax,sum)
		return max(lmax + rmax,maxsum3(1, m),maxsum3(m+1,u))

在该算法中，在每层的递归中都有O(n)次操作，总共有O(logn)层递归，即算法复杂度为O(nlogn)

#### 扫描算法

在前i个元素中，最大和子数组要么再前i-1中（我们将其存储在maxsofar中），要么其结束位置为i（我们将其存储在maxendinghere中）

思路：遍历数组，若maxendinghere加上x[i]后，结果为正值，则更新maxsofar（取maxsofar和maxendinghere中较大的一个）；若结果为负值，则直接将maxendinghere重置为0，算法4伪代码：

	maxsofar = 0
	maxendinghere = 0
	for i = [0,n)
		maxendinghere = max(maxendinghere+x[i],0)
		maxsofar = max(maxsofar,maxendinghere)

该算法的运行时间为O(n),称为线性算法。

### 后记

通过算法的不断演进，将一个O(n3)的程序优化到线性算法,如果n的值达到100 000，达到的效果是由15天解决的问题优化到5毫秒就达到相同的效果。

[这里是记录的用Java实现的各算法的实例练习](https://github.com/Zering/LeetCode/blob/master/src/main/java/com/algorithm/OneDimensionalMaxSum.java)

