---
layout:     post
title:      "101. Symmetric Tree-对称树"
subtitle:   " \"LeetCode\""
date:       2016-10-18 14:44:24 
author:     "Zering"
header-img: "img/road.jpg"
catalog: true
tags:
    - LeetCode
---

## 对称树

### 问题

问题地址：[https://leetcode.com/problems/symmetric-tree/](https://leetcode.com/problems/symmetric-tree/)

Given a binary tree, check whether it is a mirror of itself (ie, symmetric around its center).

For example, this binary tree [1,2,2,3,4,4,3] is symmetric:

	    1
	   / \
	  2   2
	 / \ / \
	3  4 4  3
But the following [1,2,2,null,3,null,3] is not:

	    1
	   / \
	  2   2
	   \   \
	   3    3

Note:
Bonus points if you could solve it both recursively and iteratively.

### 问题翻译

给一个二叉树，验证它是否是镜像对称的（即，中心对称）。

例如，二叉树[1,2,2,3,4,4,3] 是对称的：

	    1
	   / \
	  2   2
	 / \ / \
	3  4 4  3

但是，[1,2,2,null,3,null,3] 就不是：

	    1
	   / \
	  2   2
	   \   \
	   3    3

注意：
如果你能递归的、迭代的解决它将有额外的分数。

### 问题分析：

以题中的示例的最简单的对称树，可以看出最左边的左子树是和最右边的右子树相等的，然后向内收敛相等；在根据注意里面应该采用递归的方法，可以得出递归的条件是：left.left.val == right.right.val && left.right.val == right.left.val。递归结束的条件是所有的下一层子树都是null。

### Java代码示例：

	/**
	 * Definition for a binary tree node.
	 * public class TreeNode {
	 *     int val;
	 *     TreeNode left;
	 *     TreeNode right;
	 *     TreeNode(int x) { val = x; }
	 * }
	 */
	public class Solution {
	    public boolean isSymmetric(TreeNode root) {
	        if(root == null)
	            return true;
	        return isMirror(root.left,root.right);
	    }
	    public boolean isMirror(TreeNode p,TreeNode q){
	        if(p == null && q == null)
	            return true;
	        if(p == null || q == null)
	            return false;
	        return (p.val==q.val) && isMirror(p.left,q.right) && isMirror(p.right,q.left);
	    }
	}

### 后记

练习及测试的代码地址：[https://github.com/Zering/LeetCode](https://github.com/Zering/LeetCode)

SymmetricTree.java 为源码，SymmetricTreeTest.java 为测试。