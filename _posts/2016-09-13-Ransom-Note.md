---
layout:     post
title:      "383. Ransom Note-勒索信"
subtitle:   " \"LeetCode\""
date:       2016-9-13 10:08:36
author:     "Zering"
header-img: "img/road.jpg"
catalog: true
tags:
    - LeetCode
---
### 问题
问题地址：https://leetcode.com/problems/ransom-note/

Given an arbitrary ransom note string and another string containing letters from all the magazines, write a function that will return true if the ransom  note can be constructed from the magazines ; otherwise, it will return false.  

Each letter in the magazine string can only be used once in your ransom note.

Note: 
You may assume that both strings contain only lowercase letters.

	canConstruct("a", "b") -> false
	canConstruct("aa", "ab") -> false
	canConstruct("aa", "aab") -> true

### 问题翻译

任意给你一个勒索信字符串和一个包含杂志所有字母的字符串，写一个功能判断勒索信能否由杂志的字母组成。

杂志里的每一个字母只能使用一次

注意：假定所有的字母都是小写字母，示例：

	canConstruct("a", "b") -> false
	canConstruct("aa", "ab") -> false
	canConstruct("aa", "aab") -> true

### 问题分析
1. 杂志必须包含勒索信的所有字母；
2. 杂志单个字母只能使用一次；
这个问题，最简单的想法就是逐一的遍历勒索信和杂志字母，以下提供三种解法：

解法一：将ransomNote的字符串挨个遍历，每个字符再从magazine里遍历匹配，只是再创建了个byte数组，数组每个元素的索引表示magazine字符串的位置，元素值表示是否被校验过，0表示还未被校验过，非0就表示该位置已经被校验过。不过这种做法效率不高。

解法二：将ransomNote和magazine都从小到大排序，然后对ransomNote遍历，同时在magazine中匹配，如果匹配到了，则记住此时magazine中字符的索引，便于下次操作，因为都是排序的。如果直到magazine中字符已经大于ransomNote中字符了，就说明就再也匹配不到了，则表示匹配失败。

解法三：LeetCode Discuss中的最热代码，它的原理是列出了magazine的字母表，然后算出了出现个数，然后遍历ransomNote，保证有足够的字母可用，代码非常清晰。

### 问题解答（java示例）
解法一：

	public boolean canConstruct(String ransomNote, String magazine) {
        boolean ret = true;
        byte[] bytes = new byte[magazine.length()];
        for (int i = 0; i < ransomNote.length(); i++) {
            char c = ransomNote.charAt(i);
            boolean found = false;
            for (int j = 0; j < magazine.length(); j++) {
                if (bytes[j] == 0 && magazine.charAt(j) == c) {
                    bytes[j]++;
                    found = true;
                    break;
                }
            }
            if (!found) {
                ret = false;
                break;
            }
        }

        return ret;
    }

解法二：

	public boolean canConstruct(String ransomNote, String magazine) {
        boolean ret = true;
        char[] ra = ransomNote.toCharArray();
        Arrays.sort(ra);
        char[] ma = magazine.toCharArray();
        Arrays.sort(ma);

        int index = 0;
        boolean found = true;
        for (int i = 0; i < ra.length && ret; i++) {
            char ri = ra[i];
            found = false;
            for (int j = index; j < ma.length; j++) {
                if (ma[j] > ri) {
                    ret = false;
                    break;
                } else if (ma[j] == ri) {
                    index++;
                    found = true;
                    break;
                } else {
                    index++;
                }
            }
            if (!found) {
                ret = false;
                break;
            }
        }

        return ret;
    }

解法三：

	public boolean canConstruct(String ransomNote, String magazine) {
        int[] arr = new int[26];
        for (int i = 0; i < magazine.length(); i++) {
            arr[magazine.charAt(i) - 'a']++;
        }
        for (int i = 0; i < ransomNote.length(); i++) {
            if(--arr[ransomNote.charAt(i)-'a'] < 0) {
                return false;
            }
        }
        return true;
    }

### 后记
练习源码地址：[https://github.com/Zering/LeetCode](https://github.com/Zering/LeetCode)