---
layout:     post
title:      "114. Flatten Binary Tree to Linked List-将二叉树转为链表"
subtitle:   " \"LeetCode\""
date:       2017-1-9 15:04:25 
author:     "Zering"
header-img: "img/road.jpg"
catalog: true
tags:
    - LeetCode
---

## 将二叉树转为链表

### 问题

问题地址：

[114. Flatten Binary Tree to Linked List](https://leetcode.com/problems/flatten-binary-tree-to-linked-list/)

Given a binary tree, flatten it to a linked list in-place.

For example,
Given

         1
        / \
       2   5
      / \   \
     3   4   6
The flattened tree should look like:
	
	   1
	    \
	     2
	      \
	       3
	        \
	         4
	          \
	           5
	            \
	             6

### 问题翻译

给一个二叉树，将其变成一个链表结构

示例：

         1
        / \
       2   5
      / \   \
     3   4   6
改变后变为：

	   1
	    \
	     2
	      \
	       3
	        \
	         4
	          \
	           5
	            \
	             6


### 问题分析

二叉树的问题一般可以分为左子树和右子树来分析，根据例子的观察我们得出以下结论：

1. 根节点不变；
2. 改变后的所有左子树为空；
3. 链表的顺序是先序遍历的--》将左子树接在根节点的右子树上，在将原来的右子树接在现在的右子树上；

因此，我们以以下步骤来进行：

1. 记录原根节点及其左右子树；
2. 将左子树置空；
3. 将每个节点改为先序遍历的；

### Java解决方案示例

	public void flatten(TreeNode root) {
		if (root == null) return;
		
		TreeNode left = root.left;
		TreeNode right = root.right;
		
		root.left = null;
		
		flatten(left);
		flatten(right);
		
		root.right = left;
		
		TreeNode cur = root;
		while (cur.right != null) cur = cur.right;
		cur.right = right;
	}

### leetcode上的Top Solution

	private TreeNode prev = null;
	
	public void flatten(TreeNode root) {
	    if (root == null)
	        return;
	    flatten(root.right);
	    flatten(root.left);
	    root.right = prev;
	    root.left = null;
	    prev = root;
	}

### 后记

源码：[https://github.com/Zering/LeetCode](https://github.com/Zering/LeetCode)


