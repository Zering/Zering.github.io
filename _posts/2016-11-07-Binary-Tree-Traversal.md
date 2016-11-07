---
layout:     post
title:      "二叉树的遍历（总集）"
subtitle:   " \"LeetCode\""
date:       2016-11-7 16:40:21  
author:     "Zering"
header-img: "img/road.jpg"
catalog: true
tags:
    - LeetCode
---
## 二叉树的遍历

### 前言

本篇包含二叉树的先序、中序、后序、层序和之字型遍历

对应leedcode的144、94、145、102、103

**BTW：** 先、中、后 是以根节点的角度来描述的

### 前提

树的结构模型：

	public class TreeNode {
        int val;
        TreeNode left;
        TreeNode right;
        TreeNode(int x) { val = x; }
	}

### 递归实现

    /** 
     * @param root 树根节点 
     * 递归先序遍历 
     */  
    public static void preOrderRec(Node root){  
        if(root!=null){  
            System.out.println(root.value);  
            preOrderRec(root.left);  
            preOrderRec(root.right);  
        }  
    }  
    /** 
     * @param root 树根节点 
     * 递归中序遍历 
     */  
    public static void inOrderRec(Node root){  
        if(root!=null){  
            preOrderRec(root.left);  
            System.out.println(root.value);  
            preOrderRec(root.right);  
        }  
    }  
    /** 
     * @param root 树根节点 
     * 递归后序遍历 
     */  
    public static void postOrderRec(Node root){  
        if(root!=null){  
            preOrderRec(root.left);  
            preOrderRec(root.right);  
            System.out.println(root.value);  
        }  
    } 

### 非递归实现

	/** 
     * @param root 树根节点 
     * 先序遍历 
     */  
	public List<Integer> preorderTraversal(TreeNode root) {
		List<Integer> list = new LinkedList<>();
		Stack<TreeNode> nodes = new Stack<>();
		TreeNode cur = root;
		while (cur != null || !nodes.isEmpty()) {
			while (cur != null) {
				nodes.push(cur);
				list.add(cur.val);
				cur = cur.left;
			}
			cur = nodes.pop();
			cur = cur.right;
		}
		return list;
    }

	/**
	 * 中序遍历
     */
	public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> list = new LinkedList<>();
        if (root == null) {
			return list;
		}
        Stack<TreeNode> nodes = new Stack<>();
        TreeNode cur = root;
        while (cur != null || !nodes.empty()) {
			while (cur != null) {
				nodes.push(cur);
				cur = cur.left;
			}
			cur = nodes.pop();
			list.add(cur.val);
			cur = cur.right;
		}
        return list;
    }

	/**
	 * 后序遍历
     */
	public List<Integer> postorderTraversal(TreeNode root) {
    	List<Integer> list = new LinkedList<>();
    	Stack<TreeNode> nodes = new Stack<>();
    	Map<TreeNode, Boolean> map = new HashMap<>();
    	nodes.push(root);
    	while (!nodes.isEmpty()) {
			TreeNode cur = nodes.peek();
			if (cur.left != null && !map.containsKey(cur.left)) {
				cur = cur.left;
				while (cur != null) {
					if (map.containsKey(cur)) 
						break;
					else 
						nodes.push(cur);
					cur = cur.left;
				}
				continue;
			}
			if (cur.right != null && !map.containsKey(cur.right)) {
				nodes.push(cur.right);
				continue;
			}
			
			TreeNode t = nodes.pop();
			map.put(t, true);
			list.add(t.val);
		}
    	return list;
    }
    
    /**
	 * 后序遍历
     * 只保存左子树的方法
     */
    public List<Integer> postorderTraversalBetter(TreeNode root) {
		LinkedList<Integer> list = new LinkedList<>();
		Stack<TreeNode> leftChildren = new Stack<>();
		TreeNode cur = root;
		while (cur != null) {
			list.addFirst(cur.val);
			if (cur.left != null) {
				leftChildren.push(cur.left);
			}
			cur = cur.right;
			if (cur == null && !leftChildren.isEmpty()) {
				cur = leftChildren.pop();
			}
		}
		return list;
	}

	/**
	 * 层序遍历
     */
	public List<List<Integer>> levelOrder(TreeNode root) {
		List<List<Integer>> list = new LinkedList<>();
		Queue<TreeNode> queue = new LinkedList<>();
		if (root == null) {
			return list;
		}
		queue.offer(root);
		while (!queue.isEmpty()) {
			int level = queue.size();
			List<Integer> subList = new LinkedList<>();
			for (int i = 0; i < level; i++) {
				if (queue.peek().left != null) 
					queue.offer(queue.peek().left);
				if (queue.peek().right != null) 
					queue.offer(queue.peek().right);
				subList.add(queue.poll().val);
			}
			list.add(subList);
		}
		return list;
	}
	
	/**
	 * 之字形遍历
     */
	public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
		List<List<Integer>> list = new LinkedList<>();
		Queue<TreeNode> queue = new LinkedList<>();
		if (root == null) {
			return list;
		}
		queue.offer(root);
		while (!queue.isEmpty()) {
			int level = queue.size();
			List<Integer> subList = new LinkedList<>();
			for (int i = 0; i < level; i++) {
				if(queue.peek().left != null) queue.offer(queue.peek().left);
				if(queue.peek().right != null) queue.offer(queue.peek().right);
				if (list.size() % 2 == 0) {
					subList.add(queue.poll().val);
				} else {
					subList.add(0,queue.poll().val);
				}
			}
			list.add(subList);
		}
		return list;
    }

### 练习地址
[https://github.com/Zering/LeetCode](https://github.com/Zering/LeetCode)