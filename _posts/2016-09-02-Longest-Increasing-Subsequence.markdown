---
layout:     post
title:      "300.Longest Increasing Subsequence-求最长的递增子序列"
subtitle:   " \"LeetCode\""
date:       2016-09-02 13:22:00
author:     "Zering"
header-img: "img/road.jpg"
catalog: true
tags:
    - LeetCode
---

### 问题

题目地址：[https://leetcode.com/problems/longest-increasing-subsequence/](https://leetcode.com/problems/longest-increasing-subsequence/)

Given an unsorted array of integers, find the length of longest increasing subsequence.

For example,

Given [10, 9, 2, 5, 3, 7, 101, 18],
The longest increasing subsequence is [2, 3, 7, 101], therefore the length is 4. Note that there may be more than one LIS combination, it is only necessary for you to return the length.

Your algorithm should run in O(n2) complexity.

Follow up: Could you improve it to O(n log n) time complexity?

### 问题翻译
有一个无序的整型数组，请找出它的最长递增子序列的长度

示例：

一个数组[10, 9, 2, 5, 3, 7, 101, 18],它的最长递增子序列为 [2, 3, 7, 101],所以长度是4.注意，可能不止一个最长递增子序列，但我们只需要你返回它的长度。

你的算法的时间复杂度应该是O(n2).

追加：你能将时间复杂度提升到 O(n log n)吗？

### 解法一分析及java示例代码（复杂度O(n2)）
遍历整个数组，用一个相同长度数组记录每个元素下的递增子序列的长度，如果后一个元素比前一个大，那它的子序列长度就应该比前一个大1，每个元素的子序列长度初始化为1，遍历时将它与前面的每一个元素比较，然后取的其中的最大的子序列长度保存。

比如示例的[10, 9, 2, 5, 3, 7, 101, 18]

	10 前面没有元素 记录1
	9 9<10 记录保持为1
	2 比前面两个都要小 ，保持为1
	5 比前面的2大，在2记录的1基础上加1 记录为2
	3 比前面的2大，在2记录的1基础上加1 记录为2
	7 7<10，记录1；7<9,记录1；
	  7>2,在2记录的1基础上加1,比当前的记录1大，记录2；
	  7>5,在5记录的2基础上加1 比当前记录2大，记录3；...
	...（依次类推）

由上面的分析可知，是有双重循环就能得出结果，java示例如下：

	public int maxSequence(int[] nums){
		if(nums==null || nums.length==0)
        	return 0;
		int[] max = new int[nums.length];
		Arrays.fill(max, 1);
		int result = 1;
	    for(int i=0; i<nums.length; i++){
	        for(int j=0; j<i; j++){
	            if(nums[i]>nums[j]){
	                max[i]= Math.max(max[i], max[j]+1);
	            }
	        }
	        result = Math.max(max[i], result);
	    }	 
	   return result;
	}

### 解法二及java示例代码（复杂度O(n log n)）
组装一个List，伪代码如下：

	for each num in nums
     if(list.size()==0)
          add num to list
     else if(num > last element in list)
          add num to list
     else 
          replace the element in the list which is the smallest but bigger than num

List里面并不是最长递增子序列，但长度与最大递增子序列相同，其中，替换元素的过程可以用二分查找实现，使得时间复杂度为O(n log n),java代码示例如下：

	public int maxLIS(int[] nums) {
		if(nums==null || nums.length==0)
	        return 0;
		List<Integer> list = new ArrayList<Integer>();
		int i,j,mid;
		for(Integer num:nums){
			if(list.size() == 0 || num > list.get(list.size()-1))
				list.add(num);
			else {
				i = 0;
				j = list.size()-1;
				while(i < j){
					mid = (i+j)/2;
					if(list.get(mid) < num){
						i = mid+1;
					} else {
						j = mid;
					}
				}
				list.set(j, num);
			}
		}
		return list.size();
	}

### 后记
一般来说，看到题目中说明的时间复杂度为O(n2)，最能联想到的就是双重for循环，涉及到两次比较，所以解法一是比较能想到的；当然如果第算法方面比较熟练的话，应该是能较快的想到解法二的，还需要继续努力。