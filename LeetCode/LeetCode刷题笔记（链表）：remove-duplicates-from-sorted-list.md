---
title: LeetCode刷题笔记（链表）：remove-duplicates-from-sorted-list
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

Given a sorted linked list, delete all duplicates such that each element appear only once.

For example,
Given1->1->2, return1->2.
Given1->1->2->3->3, return1->2->3.

## 解题思路

简单问题不要想的太复杂，直接嵌套循环搞定喽。外循环用来遍历链表，内循环用来遍历重复元素，如果重复就一直传递指针。

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
    ListNode *deleteDuplicates(ListNode *head) {
        if(head == NULL)
            return NULL;
        ListNode *dummy = head;
        while(dummy != NULL){
            while(dummy->next != NULL && dummy->val == dummy->next->val){
                dummy->next = dummy->next->next;
            }
            dummy = dummy->next;
        }
        return head;
    }
};
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**