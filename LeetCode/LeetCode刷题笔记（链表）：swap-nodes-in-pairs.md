---
title: LeetCode刷题笔记（链表）：swap-nodes-in-pairs
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

Given a linked list, swap every two adjacent nodes and return its head.

For example,
Given1->2->3->4, you should return the list as2->1->4->3.

Your algorithm should use only constant space. You may not modify the values in the list, only nodes itself can be changed.

## 解题思路

通过这么多次的编程练习，链表的顺序交换我相信大家早已轻车熟路。本题中，我们先写一个交换相邻结点的函数swap，此函数交换输入的两个相邻链表结点之后，返回新的第一结点。然后我们开始考虑主函数，同样的新建一个指向头结点的指针（只要头结点有可能被替换，我们既需要这么做），然后遍历整个链表调用交换函数swap即可。

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
    ListNode *swap(ListNode *slow, ListNode *fast){
        slow->next = fast->next;
        fast->next = slow;
        return fast;
    }
    
    ListNode *swapPairs(ListNode *head) {
        if(head == NULL || head->next == NULL)
            return head;
        ListNode *dummy = new ListNode(0);
        dummy->next = head;
        ListNode *cur = dummy;
        for(;cur->next != NULL && cur->next->next != NULL; cur = cur->next->next)
            cur->next = swap(cur->next, cur->next->next);
        return dummy->next;
    }
};
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**