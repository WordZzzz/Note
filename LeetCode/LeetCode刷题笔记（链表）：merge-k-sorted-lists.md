---
title: LeetCode刷题笔记（链表）：merge-k-sorted-lists
comments: true
mathjax: true
categories:
  - Leetcode
tags:
  - Leetcode
  - List
---

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **代码地址：https://github.com/WordZzzz/Note/tree/master/LeetCode**
- **刷题平台：https://www.nowcoder.com/ta/leetcode**
- **题&emsp;&emsp;库：Leetcode经典编程题**
- **编&emsp;&emsp;者：WordZzzz**

----------

[toc]

## 题目描述

Merge k sorted linked lists and return it as one sorted list. Analyze and describe its complexity.

## 解题思路

之前做过一道题是对两个排序链表进行合并，这次是k个。所以，需要在原来的基础长做一些封装。

按照归并排序的思想，我们通过二分法得到中点进行递归，自下而上进行两两归并，直到只剩最后一个链表。

## C++版代码实现

```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *mergeKLists(vector<ListNode *> &lists) {
        if(lists.size() == 0)
            return NULL;
        return mergeList(lists, 0, lists.size()-1);
    }
    
    ListNode *mergeList(vector<ListNode *> &lists, int low, int high){
        if(high <= low)
            return lists[low];
        int mid = (low + high) / 2;
        ListNode *left = mergeList(lists, low, mid);
        ListNode *right = mergeList(lists, mid+1, high);
        return merge(left, right);
    }
    
    ListNode *merge(ListNode *left, ListNode *right) {
        if(left == NULL)
            return right;
        if(right == NULL)
            return left;
        
        ListNode *dummy = new ListNode(0);
        ListNode *cur = dummy;
        while(left != NULL && right != NULL){
            if(left->val < right->val){
                cur->next = left;
                left = left->next;
            }
            else{
                cur->next = right;
                right = right->next;
            }
            cur = cur->next;
        }
        cur->next = left ? left : right;
        return dummy->next;
    }
};
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**