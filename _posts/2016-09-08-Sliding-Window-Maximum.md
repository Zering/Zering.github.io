---
layout:     post
title:      "239.Sliding Window Maximum-滑动窗口最大值"
subtitle:   " \"LeetCode\""
date:       2016-9-8 10:18:37 
author:     "Zering"
header-img: "img/road.jpg"
catalog: true
tags:
    - LeetCode
---
### 问题
问题地址：[https://leetcode.com/problems/sliding-window-maximum/](https://leetcode.com/problems/sliding-window-maximum/)

Given an array nums, there is a sliding window of size k which is moving from the very left of the array to the very right. You can only see the k numbers in the window. Each time the sliding window moves right by one position.

For example,
Given nums = [1,3,-1,-3,5,3,6,7], and k = 3.

	Window position                Max
	---------------               -----
	[1  3  -1] -3  5  3  6  7       3
	 1 [3  -1  -3] 5  3  6  7       3
	 1  3 [-1  -3  5] 3  6  7       5
	 1  3  -1 [-3  5  3] 6  7       5
	 1  3  -1  -3 [5  3  6] 7       6
	 1  3  -1  -3  5 [3  6  7]      7

Therefore, return the max sliding window as [3,3,5,5,6,7].

Note: 
You may assume k is always valid, ie: 1 ≤ k ≤ input array's size for non-empty array.

Follow up:
Could you solve it in linear time?

Hint:

1. How about using a data structure such as deque (double-ended queue)?
2. The queue size need not be the same as the window’s size.
3. Remove redundant elements and the queue should store only elements that need to be considered.

### 问题翻译
给定一个数组，有一个长度为k的滑动窗口，从数组的最左边滑动到最右边。你只能看到窗口内的k给数字；每次向右边滑动一个数字

示例，给定数组nums=[1,3,-1,-3,5,3,6,7],k=3

	Window position                Max
	---------------               -----
	[1  3  -1] -3  5  3  6  7       3
	 1 [3  -1  -3] 5  3  6  7       3
	 1  3 [-1  -3  5] 3  6  7       5
	 1  3  -1 [-3  5  3] 6  7       5
	 1  3  -1  -3 [5  3  6] 7       6
	 1  3  -1  -3  5 [3  6  7]      7

返回窗口内的最大值为[3,3,5,5,6,7].

注意：你应该假定k值总是有效的，也就是说1 ≤ k ≤ 输入非空数组的长度

追加：你能在线性的时间内得出结果吗？（时间复杂度为O(n)）

提示：

1. 用一个双向队列怎么样？
2. 列表的长度不一定要和窗口相同
3. 去掉多余的数据，队列只需要保存需要考虑的元素

### 问题分析
根据提示，用双向队列保存数字的下标，遍历整个数组

	以示例分析
	i=0:直接加到队列
	i=1:比较nums[1]和队列里面的元素，移除所有比nums[1]小的值
	i=2:同上比较，此时i+1=k 即窗口马上就要移动了，此时队列里面应该只有一个值：即前k里面最大的值，将它添加到要返回的结果数组里面
	i=3:同样的进行比较,此时窗口已经移过了下标为0的元素，如果0还保留在队列，应该将他移除，
		即当i+1-k>list.getFirst时,将list.removeFirst();
		(本例中不存在这种状况，但是例如第一个元素为4，那就一定要移除他，否则会影响到后面的判断)
	...

### 问题解答（java示例）
	public int[] maxSlidingWindow(int[] nums, int k) {
        if(k == 0 || k > nums.length)
        	return new int[0];
        int[] result = new int[nums.length-k+1];
        
        LinkedList<Integer> list = new LinkedList<>();
        for(int i = 0;i < nums.length;i++){
        	while(!list.isEmpty() && nums[i] >= nums[list.getLast()]){
        		list.removeLast();
        	}
        	list.addLast(i);
        	if(i+1-k > list.getFirst()){
        		list.removeFirst();
        	}
        	if(i+1 >= k)
        		result[i+1-k] = nums[list.getFirst()];
        }
        return result;
    }

### 后记
练习源码地址：[https://github.com/Zering/LeetCode](https://github.com/Zering/LeetCode)